- li is a semantic HTML tag so screenreaders tell that and read what is inside
- need a role="tab" or role="list"
- need  tabIndex
- need click handler

---

```jsx
import React, { useEffect, useRef, useState } from "react";
import "./style/style.scss";
import { getTutorialType } from "../utils/help";
import TUTORIAL from "../constants/labels";
import { ReactComponent as STANDARD } from "./imgs/STANDARD.svg";
import { ReactComponent as AD } from "./imgs/AD.svg";
import { ReactComponent as LIS } from "./imgs/LIS.svg";

const Card = ({
	vod,
	vodIndex = -2,
	handleCardClick,
	liCss,
	disableClick = false,
}) => {
	const [type, setType] = useState(null);
	const ref = useRef(null);

	const selectSvg = () => {
		switch (type) {
			case TUTORIAL.tutorialType.STANDARD:
				return <STANDARD />;
			case TUTORIAL.tutorialType.AD:
				return <AD />;
			case TUTORIAL.tutorialType.LIS:
				return <LIS />;
			default:
				return null;
		}
	};

	const handleKeyDown = ({ key, keyCode }) => {
		if (key === "Enter" && keyCode === 13 && !disableClick) {
			handleCardClick();
		}
	};

	useEffect(() => {
		setType(getTutorialType(vod));
	}, [vod]);

	useEffect(() => {
		if (vodIndex + 1 === 1 && ref && !disableClick) {
			ref.current.focus();
			console.log(`🧊 ~ ref: `, ref);
		}
	}, [ref, vodIndex, disableClick]);

	return (
		<>
			<li
				role="tab"
				tabIndex={vodIndex + 1}
				className={`card ${liCss} disabled-${disableClick}`}
				onClick={!disableClick ? handleCardClick : () => {}}
				onKeyDown={handleKeyDown}
				ref={ref}
			>
				   
				<div className="img-wrapper">
					{selectSvg()}       
					<h1 className="img-title">{TUTORIAL.typeLabels[type]}</h1>       
				</div>
				 
			</li>
		</>
	);
};

export default Card;
```
