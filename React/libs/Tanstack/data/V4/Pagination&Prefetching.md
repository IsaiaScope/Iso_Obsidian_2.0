_Pagination_

- Track current page in component state (current Page)
- Use query keys that include the page number ["posts", currentPage]
- User clicks "next page" or "previous page" button
  - update currentPage state
  - fire off new query

---

_Prefetching_

- [DOC of Prefetching](https://tanstack.com/query/latest/docs/react/reference/QueryClient?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Freference%2FQueryClient#queryclientprefetchquery)
- Prefetch
  - adds data to cache
  - automatically stale (configurable)
  - shows while re-fetching
    - as long as cache hasn't expired!
- Prefetching can be used for any anticipated data needs
  - not just pagination!

---

- Pagination with Prefetching:

```jsx
import { useEffect, useState } from "react";
import { useQuery, useQueryClient } from "@tanstack/react-query";

import { PostDetail } from "./PostDetail";

const maxPostPage = 10;

async function fetchPosts(pageNum) {
	const response = await fetch(
		`https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${pageNum}`
	);
	return response.json();
}

export function Posts() {
	const [currentPage, setCurrentPage] = useState(1);
	const [selectedPost, setSelectedPost] = useState(null);

	const queryClient = useQueryClient();

	useEffect(() => {
		if (currentPage < maxPostPage) {
			const nextPage = currentPage + 1;
			queryClient.prefetchQuery(["posts", nextPage], () =>
				fetchPosts(nextPage)
			);
		}
	}, [currentPage, queryClient]);

	const { data, isError, error, isLoading } = useQuery(
		["posts", currentPage],
		() => fetchPosts(currentPage),
		{
			staleTime: 2000,
			keepPreviousData: true,
		}
	);
	if (isLoading) return <h3>Loading...</h3>;
	if (isError)
		return (
			<>
				<h3>Oops, something went wrong</h3>
				<p>{error.toString()}</p>
			</>
		);

	return (
		<>
			<ul>
				{data.map((post) => (
					<li
						key={post.id}
						className="post-title"
						onClick={() => setSelectedPost(post)}
					>
						{post.title}
					</li>
				))}
			</ul>
			<div className="pages">
				<button
					disabled={currentPage <= 1}
					onClick={() => {
						setCurrentPage((previousValue) => previousValue - 1);
					}}
				>
					Previous page
				</button>
				<span>Page {currentPage}</span>
				<button
					disabled={currentPage >= maxPostPage}
					onClick={() => {
						setCurrentPage((previousValue) => previousValue + 1);
					}}
				>
					Next page
				</button>
			</div>
			<hr />
			{selectedPost && <PostDetail post={selectedPost} />}
		</>
	);
}
```
