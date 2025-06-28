## データ取得とフェッチ

### 1. TanStack Query の活用

- クライアントサイドのデータ取得には TanStack Query を使用
- キャッシュとデータの再取得戦略を適切に設定
- エラーハンドリングと読み込み状態を一貫して管理

```typescript
// hooks/useUsers.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { userApi } from '@/lib/api/userApi';
import type { User } from '@/db/schema/users';

export function useUsers() {
  const queryClient = useQueryClient();

  const usersQuery = useQuery<User[]>({
    queryKey: ['users'],
    queryFn: userApi.getAll,
  });

  const createUserMutation = useMutation({
    mutationFn: userApi.create,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });

  return {
    users: usersQuery.data || [],
    isLoading: usersQuery.isLoading,
    error: usersQuery.error,
    createUser: createUserMutation.mutate,
    isCreating: createUserMutation.isPending,
  };
}
```

### 2. API クライアント

- 型安全な API クライアントを実装
- Fetch API をベースにしたラッパー
- エラーハンドリングとレスポンス処理の一貫性を確保

```typescript
// lib/api/apiClient.ts
import { z } from 'zod';

const apiResponseSchema = z.object({
  data: z.unknown().optional(),
  error: z.object({
    message: z.string(),
  }).optional(),
});

type ApiResponse<T> = {
  data?: T;
  error?: { message: string };
};

export async function apiClient<T>(
  url: string,
  options?: RequestInit
): Promise<ApiResponse<T>> {
  try {
    const response = await fetch(url, {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options?.headers,
      },
    });

    const jsonData = await response.json();
    const parsedResponse = apiResponseSchema.parse(jsonData);

    if (!response.ok) {
      return { error: { message: parsedResponse.error?.message || 'Unknown error' } };
    }

    return { data: parsedResponse.data as T };
  } catch (error) {
    if (error instanceof Error) {
      return { error: { message: error.message } };
    }
    return { error: { message: 'Unknown error occurred' } };
  }
}
