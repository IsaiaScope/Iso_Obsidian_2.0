- _React Query Mutations_
  - introduced mutations back in Blog-em Ipsum
  - Here, use in a more realistic way (server will update!)
    - Invalidate query on mutation so data is purged from the cache
    - Update cache with data returned from the server after mutation
    - ptimistic update (assume mutation will succeed, rollback if not)
- _Global Fetching / Error_
  - Very similar to queries
  - Errors
    - set onError callback in mutations property of query client defaultoptions
  - Loading indicator
    - useIsMutating is analogous to useIsFetching
    - Update Loading component to show on isMutating

---

- _useMutation_
  - very similar to useQuery!
  - Differences
    - no cache data
    - no retries
    - no refetch
    - no isLoading vs isFetching
    - returns mutate function which actually runs mutation
    - onMutate callback (useful for optimistic queries!)
  - reference:
    - https:/react-query.tanstack.com/reference/useMutation
    - https://react-query.tanstack.com/guides/mutations
- _Query Key Prefix for Appointments_
  - For user appointments
    - [ querykeys.appointments, queryKeys.user, user?.id ]
  - For appointments
    - [queryKeys. appointments, queryMonthYear.year, queryMonthYear. month]
  - Pass [ queryKeys. appointments ] as prefix to invalidateQueries
    - invalidates both
  - For more complicated apps, use functions to create query keys to ensure consistency
  - reference
    - https://react-query.tanstack.com/guides/query-keys
    - https://react-query.tanstack.com/guides/query-invalidation#query-matching-with-invalidatequeries
- _Update Cache from Mutation Response_
  - New custom hook usePatchUser
    - update query cache with results from mutation server call
  - Use the handy updateUser function from useUser
    - will update query cache and localStorage
  - reference:
    - https://react-querytanstack.com/guides/updates-from-mutation-responses

---

- [[mutate-function-from-custom-hook.png| IMG of returning mutate function from custom hook]]

```tsx
import {
  UseMutateFunction,
  useMutation,
  useQueryClient,
} from '@tanstack/react-query';

import { Appointment } from '../../../../../shared/types';
import { axiosInstance } from '../../../axiosInstance';
import { queryKeys } from '../../../react-query/constants';
import { useCustomToast } from '../../app/hooks/useCustomToast';
import { useUser } from '../../user/hooks/useUser';

async function setAppointmentUser(
  appointment: Appointment,
  userId: number | undefined
): Promise<void> {
  if (!userId) return;
  const patchOp = appointment.userId ? 'replace' : 'add';
  const patchData = [{ op: patchOp, path: '/userId', value: userId }];
  await axiosInstance.patch(`/appointment/${appointment.id}`, {
    data: patchData,
  });
}

export function useReserveAppointment(): UseMutateFunction<
  void,
  unknown,
  Appointment,
  unknown
> {
  const { user } = useUser();
  const toast = useCustomToast();
  const queryClient = useQueryClient();

  const { mutate } = useMutation(
    (appointment: Appointment) => setAppointmentUser(appointment, user?.id),
    {
      onSuccess: () => {
        queryClient.invalidateQueries([queryKeys.appointments]);
        toast({
          title: 'You have reserved the appointment!',
          status: 'success',
        });
      },
    }
  );

  return mutate;
}
```

---
