# Write a React component that renders a dynamic data table with sortable columns and pagination. How would you style the table to ensure that it meets the design specifications and is easy to use on both desktop and mobile devices?
  
```jsx
import React, { useState } from 'react';
import './DataTable.css';

// Define a functional component named "DataTable" that accepts two props: "data" and "columns"
const DataTable = ({ data, columns }) => {
  // Use the "useState" hook to define four state variables: "sortColumn", "sortOrder", "currentPage", and "pageSize"
  const [sortColumn, setSortColumn] = useState(null);
  const [sortOrder, setSortOrder] = useState('asc');
  const [currentPage, setCurrentPage] = useState(1);
  const [pageSize, setPageSize] = useState(10);

  // Use the "sortData" function to sort the "data" array based on the "sortColumn" and "sortOrder" state variables
  const sortedData = sortData(data, sortColumn, sortOrder);

  // Use the "paginateData" function to slice the "sortedData" array based on the "currentPage" and "pageSize" state variables
  const paginatedData = paginateData(sortedData, currentPage, pageSize);

  // Calculate the total number of pages based on the length of the "sortedData" array and the "pageSize" state variable
  const totalPages = Math.ceil(sortedData.length / pageSize);

  // Define an event handler named "handleSort" that takes a "column" parameter
  const handleSort = (column) => {
    // If the "sortColumn" state variable is equal to the clicked "column", toggle the "sortOrder" state variable
    if (sortColumn === column) {
      setSortOrder(sortOrder === 'asc' ? 'desc' : 'asc');
    } else {
      // Otherwise, set the "sortColumn" state variable to the clicked "column" and reset the "sortOrder" state variable to 'asc'
      setSortColumn(column);
      setSortOrder('asc');
    }
    // Reset the "currentPage" state variable to 1 to show the first page of the sorted and paginated data
    setCurrentPage(1);
  };

  // Define an event handler named "handlePageSizeChange" that takes an "event" parameter
  const handlePageSizeChange = (event) => {
    // Set the "pageSize" state variable to the selected page size and reset the "currentPage" state variable to 1
    setPageSize(Number(event.target.value));
    setCurrentPage(1);
  };
  return (
  <div className="DataTable">
    <table>
      <thead>
        <tr>
          {columns.map((column) => (
            <th key={column.field} onClick={() => handleSort(column.field)}>
              {column.title}
              {sortColumn === column.field && (
                <span>{sortOrder === 'asc' ? ' ▲' : ' ▼'}</span>
              )}
            </th>
          ))}
        </tr>
      </thead>
      <tbody>
        {paginatedData.map((row) => (
          <tr key={row.id}>
            {columns.map((column) => (
              <td key={`${row.id}-${column.field}`}>{row[column.field]}</td>
            ))}
          </tr>
        ))}
      </tbody>
    </table>
    <div className="Pagination">
      <span>
        Showing {pageSize * (currentPage - 1) + 1}-
        {Math.min(pageSize * currentPage, sortedData.length)} of {sortedData.length}
      </span>
      <span>
        Page {currentPage} of {totalPages}
      </span>
      <select value={pageSize} onChange={handlePageSizeChange}>
        {[10, 25, 50, 100].map((size) => (
          <option key={size} value={size}>
            Show {size}
          </option>
        ))}
      </select>
      <div>
        <button onClick={() => setCurrentPage(1)} disabled={currentPage === 1}>
          {'<<'}
        </button>
        <button onClick={() => setCurrentPage(currentPage - 1)} disabled={currentPage === 1}>
          {'<'}
        </button>
        <button onClick={() => setCurrentPage(currentPage + 1)} disabled={currentPage === totalPages}>
          {'>'}
        </button>
        <button onClick={() => setCurrentPage(totalPages)} disabled={currentPage === totalPages}>
          {'>>'}
        </button>
      </div>
    </div>
  </div>
);
```