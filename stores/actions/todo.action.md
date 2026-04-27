# Todo Store actions

## todo/fetchTodos

Rough explanation:

- Fetch GET /todo
- Let `response` is a JSON parsed response
- Commit mutation todo/TODO_SET_LIST with data: `response.data`

Used by:

- [web_js] home/lecturer.js
- [web_js] home/student.js
- [web_js] todo/list.js
