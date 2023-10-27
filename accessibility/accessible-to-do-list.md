<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css" />
    <title>Accessible to do list</title>
</head>

<body>
    <div class="container">
        <div class="content">
            <h1 tabindex="-1" id="heading">To do list</h1>
            <form action="index.html" method="post" id="to_do_form">
                <label for="todo" id="main_label">Add a new task to your list (required)</label>
                <div>
                    <input type="text" class="task_input" aria-describedby="example" id="todo" required>
                    <input type="submit" value="Add" class="add_task">
                </div>
                <div class="example" id="example">
                    Example: Feed The Chicken
                </div>
            </form>
            <ul id="list" class="list">
            </ul>
            <div role="status" aria-live="polite" id="sc_feedback" class="sc_feedback">
                <!-- Area to be pronounceable by screen readers -->
            </div>
        </div>
    </div>
    <script src="script.js"></script>
</body>

</html>

---

const to_do_input = document.querySelector('#todo');
const list = document.querySelector('#list');
const to_do_form = document.querySelector('#to_do_form');
const sc_feedback = document.querySelector('#sc_feedback');
const heading = document.querySelector('#heading');

/*
  Execute functions on submit of the form
*/
to_do_form.addEventListener('submit', function (event) {
  event.preventDefault();
  let task = to_do_input.value;
  addTaskToDOM(task);
  removeValue(to_do_input);
  screenReaderFeedback(task);
});

/*
  We are using Event Bubbling to listen to when the <ul> element
  that keeps the tasks is clicked. When it is clicked, if the click
  originates on the delete button, then we execute the delete 
  task functionality. 
*/
list.addEventListener('click', function (event) {
  if (hasClassName(event.target, 'delete_task')) {
    const li = event.target.closest('li'); // Find the parent <li> of the clicked delete button
    const taskName = event.target.previousElementSibling.textContent; // Get the task text
    deleteTask(li);
    moveFocus(heading);
    screenReaderFeedback(taskName, 'removed');
  }
});

function deleteTask(theTarget) {
  list.removeChild(theTarget);
}

/*
  We generate a new <li>, <input type="checkbox">, and <button>
  each time a task is added to the DOM. 
*/
function addTaskToDOM(task) {
  let newID = generateID();
  let taskItem = createElement('li', '', list, ['class', 'task']);
  let theCheckbox = createElement('input', null, taskItem, [
    'type',
    'checkbox',
  ]);
  let label = createElement('label', task, taskItem, ['for', newID]);
  theCheckbox.setAttribute('id', newID);
  let deleteButton = createElement('button', 'Delete Task', taskItem, [
    'class',
    'delete_task',
  ]);
}

function createElement(tagName, textNode, parent, attribute = null) {
  let node = document.createElement(tagName);
  if (textNode != null) {
    let customTextNode = document.createTextNode(textNode);
    node.appendChild(customTextNode);
  }
  if (attribute != null) {
    node.setAttribute(attribute[0], attribute[1]);
  }

  parent.appendChild(node);
  return node;
}

function removeValue(input) {
  input.value = '';
}
/*
  Sighted users are able to see when a task is added, but screen reader
  users are more likely to be blind, so we want to make the 
  screen reader voice the actions. 
  We use the #sc_feedback element in the DOM to populate 
  the text which would be voiced
*/
function screenReaderFeedback(task, feedback = 'added') {
  sc_feedback.textContent = `${task} ${feedback}.`;
}

/*
  We want the checkbox to go before the label,
  so we need to connect the labels to the inputs
  with IDs. 
*/
function generateID() {
  let idPrefix = 'task_num_';
  let tasks = document.querySelectorAll('#list > li');
  if (tasks.length == 0) {
    return `${idPrefix}0`;
  }
  return idPrefix + tasks.length;
}

function moveFocus(element) {
  element.focus();
}
function hasClassName(element, className) {
  /*
    We are using classList.contains() instead
    of element.className because an element may
    have multiple classes
  */
  if (element.classList.contains(className)) {
    return true;
  }
  return false;
}

---

.list {
  display: inline-block;
  padding-left: 0;
}

.focus {
  border-bottom: 3px dotted red;
}

.task label,
.task button,
.task input {
  margin-right: 7px;
}

.task {
  margin-bottom: 10px;
}

.task:last-child {
  font-weight: bold;
}

.task_input {
  height: 39px;
}
.add_task {
  height: 44px;
  width: 44px;
}
.container {
  max-width: 1400px;
  margin: auto;
  display: flex;
}

.content {
  margin: auto;
}

.sc_feedback {
  font-weight: bold;
}