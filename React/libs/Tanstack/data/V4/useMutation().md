- _Mutation_: making a network call that changes data on the server
  - jsonplaceholder API doesn't change server
  - go through the mechanics of making the change
- Day Spa app will demonstrate showing changes to user:
  - Optimistic updates (assume change will happen)
  - Update React Query cache with data returned from the server
  - Trigger re-fetch of relevant data (invalidation)
- [DOC of useMutation()](https://tanstack.com/query/latest/docs/react/guides/mutations?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Fguides%2Fmutations)
- Similar to useQuery, but:
  - returns mutate function
  - doesn't need query key
  - isLoading but no isFetching
  - by default, no retries (configurable!)

---

```jsx
import { useMutation, useQuery } from "@tanstack/react-query";
import { useEffect } from "react";

async function fetchComments(postId) {
	const response = await fetch(
		`https://jsonplaceholder.typicode.com/comments?postId=${postId}`
	);
	return response.json();
}

async function deletePost(postId) {
	const response = await fetch(
		`https://jsonplaceholder.typicode.com/postId/${postId}`,
		{ method: "DELETE" }
	);
	return response.json();
}

async function updatePost(postId) {
	const response = await fetch(
		`https://jsonplaceholder.typicode.com/postId/${postId}`,
		{ method: "PATCH", data: { title: "REACT QUERY FOREVER!!!!" } }
	);
	return response.json();
}

export function PostDetail({ post }) {
	const { data, isLoading, isError, error } = useQuery(
		["comments", post.id],
		() => fetchComments(post.id)
	);

	const deleteMutation = useMutation((postId) => deletePost(postId));
	const updateMutation = useMutation((postId) => updatePost(postId));

	// clear messages when a new post is selected
	// reference: https://www.udemy.com/course/learn-react-query/learn/#questions/17213546/
	useEffect(() => {
		updateMutation.reset();
		deleteMutation.reset();
		// can't include updateMutation and deleteMutation in the dependencies
		// because the function updates them -- so there would be an infinite loop!
		// eslint-disable-next-line react-hooks/exhaustive-deps
	}, [post.id]);

	if (isLoading) {
		return <h3>Loading!</h3>;
	}

	if (isError) {
		return (
			<>
				<h3>Error</h3>
				<p>{error.toString()}</p>
			</>
		);
	}

	return (
		<>
			<h3 style={{ color: "blue" }}>{post.title}</h3>
			<button onClick={() => deleteMutation.mutate(post.id)}>Delete</button>
			<button onClick={() => updateMutation.mutate(post.id)}>
				Update title
			</button>
			{deleteMutation.isError && (
				<p style={{ color: "red" }}>Error deleting the post</p>
			)}
			{deleteMutation.isLoading && (
				<p style={{ color: "purple" }}>Deleting the post</p>
			)}
			{deleteMutation.isSuccess && (
				<p style={{ color: "green" }}>Post has (not) been deleted</p>
			)}
			{updateMutation.isError && (
				<p style={{ color: "red" }}>Error updating the post</p>
			)}
			{updateMutation.isLoading && (
				<p style={{ color: "purple" }}>Updating the post</p>
			)}
			{updateMutation.isSuccess && (
				<p style={{ color: "green" }}>Post has (not) been updated</p>
			)}
			<p>{post.body}</p>
			<h4>Comments</h4>
			{data.map((comment) => (
				<li key={comment.id}>
					{comment.email}: {comment.body}
				</li>
			))}
		</>
	);
}
```
