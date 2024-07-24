import React, { useState } from 'react';
import './styles.css'; // Import your CSS file for styling

const TodoList = () => {
  const [todos, setTodos] = useState([]);

  const addTodo = (todoText, dueDate) => {
    const newTodo = { id: todos.length + 1, text: todoText, completed: false, dueDate };
    setTodos([...todos, newTodo]);
  };

  const toggleComplete = (id) => {
    const updatedTodos = todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    );
    setTodos(updatedTodos);
  };

  const deleteTodo = (id) => {
    const filteredTodos = todos.filter(todo => todo.id !== id);
    setTodos(filteredTodos);
  };

  return (
    <div className="todo-list-container">
      <h2>Todo List</h2>
      <ul className="todo-list">
        {todos.map(todo => (
          <li key={todo.id} className={`todo-item ${todo.completed ? 'completed' : ''}`}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleComplete(todo.id)}
            />
            <span className="todo-text">
              {todo.text} {todo.dueDate && <span className="due-date">Due: {new Date(todo.dueDate).toLocaleDateString()}</span>}
            </span>
            <button className="delete-btn" onClick={() => deleteTodo(todo.id)}>❌</button>
          </li>
        ))}
      </ul>
      <AddTodoForm addTodo={addTodo} />
    </div>
  );
};

const AddTodoForm = ({ addTodo }) => {
  const [todoText, setTodoText] = useState('');
  const [dueDate, setDueDate] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (todoText.trim()) {
      addTodo(todoText.trim(), dueDate);
      setTodoText('');
      setDueDate('');
    }
  };

  return (
    <form onSubmit={handleSubmit} className="add-todo-form">
      <input
        type="text"
        value={todoText}
        onChange={(e) => setTodoText(e.target.value)}
        placeholder="Add a new todo"
        className="todo-input"
      />
      <input
        type="date"
        value={dueDate}
        onChange={(e) => setDueDate(e.target.value)}
        className="due-date-input"
      />
      <button type="submit" className="add-btn">➕ Add Todo</button>
    </form>
  );
};

export default TodoList;
