---
options:
  - "1"
  - "3"
selected: 1
rating: 3
rte:
  - trash
  - ok
bind_target: false
status: done
---

# Todos For {{title}}

## FilmBank [JIRA](https://fincons.atlassian.net/jira/software/projects/DDS/boards/9/timeline)

|                     DONE                      | Ticket | Desc |
| :-------------------------------------------: | :----: | :--: |
| <input type="checkbox" unchecked id="abde39"> |  []()  |      |

---

## RTE [JIRA](https://ott-jira.finconsgroup.com/secure/RapidBoard.jspa?rapidView=1&projectKey=RTEBB&view=planning.nodetail&quickFilter=1)

```meta-bind


INPUT[toggle(offValue(in progress), onValue(done)):status]



```

---

Display: <select> <option>inline</option> <option>block</option> <option>flex</option> <option>grid</option> </select> ^3ff051

|                     DONE                      | Ticket | Desc | Project | Time to Do |  Complete In  |
| :-------------------------------------------: | :----: | ---- | :-----: | :--------: | :-----------: |
| <input type="checkbox" unchecked id="64f445"> |  []()  |      |         |            | ![[#^3ff051]] |

---

```meta-bind

INPUT[inlineListSuggester(

option(trash),

option(bad),

option(ok),

option(good),

option(great)

):rte]



```
