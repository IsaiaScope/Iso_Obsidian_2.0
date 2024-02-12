---
tags:
  - JavaScript
---

# DOM Component

good way to create DOM element

```js
/**
 * @param {string} tag
 * @param {object} attrs
 * @param {array} children
 * @returns {HTMLElement}
 * @description Create an HTML element with attributes and children
 * @example
 * const elm = createElm('div', { class: 'foo' }, [createElm('p', {}, ['Hello world!'])]);
 * // elm = <div class="foo"><p>Hello world!</p></div>
 */
export function component(tag, { attrs = {}, events = {}, children = [] }) {
  const elm = document.createElement(tag);
  Object.entries(attrs).forEach(([key, value]) => {
    elm.setAttribute(key, value);
  });
  Object.entries(events).forEach(([key, value]) => {
    elm[key] = value;
  });
  children.forEach((child) => {
    if (typeof child === "string") {
      child = document.createTextNode(child);
    }
    elm.appendChild(child);
  });
  elm.updateChildren = function (newChildren) {
    elm.innerHTML = "";
    newChildren.forEach((child) => {
      elm.appendChild(child);
    });
  };
  return elm;
}
```

---

## Implementation Example

```js
export const inputRange = ({
	defaultMin = "10",
	defaultMax = "90",
	inputOne = {
		oninput: function () {} // arrow function too,
		onchange: function () {},
	},
	inputTwo = {
		oninput: function () {},
		onchange: function () {},
	},
}) => {
	const outputOne = component("span", {
		attrs: {
			class: "output-one",
		},
		children: [`${defaultMin}%`],
	});

	const outputTwo = component("span", {
		attrs: {
			class: "output-two",
		},
		children: [`${defaultMax}%`],
	});

	const inclRange = component("span", {
		attrs: {
			class: "incl-range",
			style: `width:${defaultMax - defaultMin}%; left: ${defaultMin}%;`,
		},
	});

	const fullRange = component("span", {
		attrs: {
			class: "full-range",
		},
	});

	const rangeOne = component("input", {
		attrs: {
			class: "fnc-input-range range-one",
			name: "rangeOne",
			value: defaultMin,
			max: "100",
			type: "range",
			step: "1",
		},
		events: {
			oninput: inputOne.oninput,
			onchange: inputOne.onchange,
		},
	});

	const rangeTwo = component("input", {
		attrs: {
			class: "fnc-input-range range-two",
			name: "rangeTwo",
			value: defaultMax,
			max: "100",
			type: "range",
			step: "1",
		},

		events: {
			oninput: inputTwo.oninput,
			onchange: inputTwo.onchange,
		},
	});

	const wrapper = component("section", {
		attrs: {
			class: "fnc-input-range-wrapper",
		},
		children: [outputOne, fullRange, inclRange, rangeOne, rangeTwo, outputTwo],
	});
	return wrapper;
};
```

---
