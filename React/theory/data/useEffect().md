


|What is an "Effect" (or a "Side Effect")? |
:----------------:|:-------------|
| Main Job: Render Ul & React to User Input  |  Side Effects: Anything Else  |
| 	Evaluate & Render JSX,	Manage State & Props,	React to (User) Events & Input,	Re-evaluate Component upon State &	Prop Changes  | Store Data in Browser Storage, Send Http Requests to Backend Servers, Set & Manage Timers |
| This all is "baked into" React via thc "tools" and features covered in this course (i.e. useState() Hook. Props etc).  |  these tasks must happen outside normal component evaluation and render cycle — especially since they might block/delay rendering (e.g. Http request) |









