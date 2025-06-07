<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Data Logger</title>
    <style>
        :root {
            --primary: #4a6fa5;
            --secondary: #166088;
            --accent: #4fc3f7;
            --light: #f8f9fa;
            --dark: #212529;
            --success: #28a745;
            --warning: #ffc107;
            --danger: #dc3545;
            --info: #17a2b8;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: var(--dark);
            background-color: #f5f7fa;
            margin: 0;
            padding: 0;
        }
        
        .container {
            max-width: 900px;
            margin: 20px auto;
            padding: 20px;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        
        .header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 15px 20px;
            border-radius: 8px;
            margin-bottom: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        
        .header h1 {
            margin: 0;
            font-size: 2rem;
            text-align: center;
        }
        
        .tabs {
            display: flex;
            border-bottom: 1px solid #ddd;
            margin-bottom: 20px;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
            font-weight: 500;
            color: #555;
        }
        
        .tab.active {
            border-bottom-color: var(--primary);
            color: var(--primary);
            font-weight: 600;
        }
        
        .tab-content {
            display: none;
            padding: 10px 0;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .form-group {
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        .form-label {
            min-width: 80px; /* Aligns labels */
            font-weight: 600;
            color: var(--secondary);
        }
        
        .form-control, .form-select {
            flex: 1;
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
            box-sizing: border-box;
            min-width: 150px; /* Prevent shrinking too much */
        }
        
        .form-control:focus, .form-select:focus {
            border-color: var(--accent);
            outline: none;
            box-shadow: 0 0 0 3px rgba(79, 195, 247, 0.2);
        }
        
        .btn {
            display: inline-block;
            padding: 8px 16px;
            border-radius: 4px;
            background-color: var(--primary);
            color: white;
            border: none;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
        }
        
        .btn:hover {
            background-color: var(--secondary);
            transform: translateY(-1px);
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        
        .btn-danger {
            background-color: var(--danger);
        }
        
        .btn-danger:hover {
            background-color: #c82333;
        }

        .btn-success {
            background-color: var(--success);
        }

        .btn-success:hover {
            background-color: #218838;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #eee;
        }
        
        th {
            background-color: var(--light);
            font-weight: 600;
            color: var(--secondary);
        }
        
        tr:hover {
            background-color: #f8f9fa;
        }

        td[contenteditable="true"]:focus {
            outline: 2px solid var(--accent);
            background-color: #e6f7ff;
        }

        .alert {
            padding: 10px;
            margin-bottom: 15px;
            border-radius: 4px;
            font-size: 0.9em;
        }

        .alert-success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .alert-danger {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .info-text {
            font-size: 0.9em;
            color: #6c757d;
            margin-top: 10px;
        }

        @media (max-width: 600px) {
            .form-group {
                flex-direction: column;
                align-items: flex-start;
            }
            .form-label {
                width: 100%;
            }
            .form-control, .form-select {
                width: 100%;
            }
            table {
                display: block;
                overflow-x: auto;
            }
            th, td {
                white-space: nowrap; /* Prevents text wrap in table cells */
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Simple Data Logger</h1>
        </div>
        
        <div class="tabs">
            <div class="tab active" data-tab="quickEntry">Quick Entry</div>
            <div class="tab" data-tab="allEntries">All Entries</div>
            <div class="tab" data-tab="manageOptions">Manage Options</div>
        </div>
        
        <div id="quickEntry" class="tab-content active">
            <h2>Log New Entry</h2>
            <div id="quickEntryAlert" class="alert" style="display:none;"></div>
            
            <div class="form-group">
                <label for="entrySelection" class="form-label">Select Item:</label>
                <select id="entrySelection" class="form-select"></select>
            </div>
            
            <div class="form-group">
                <label for="entryDate" class="form-label">Date:</label>
                <input type="date" id="entryDate" class="form-control">
            </div>
            
            <div class="form-group">
                <label for="entryTime" class="form-label">Time:</label>
                <input type="time" id="entryTime" class="form-control">
            </div>
            
            <div class="form-group">
                <label for="entryAmount" class="form-label">Amount:</label>
                <input type="number" id="entryAmount" class="form-control" value="0">
            </div>
            
            <button id="logEntryBtn" class="btn btn-primary">Log Entry</button>
            <p class="info-text">Select an item, optionally adjust date/time/amount, and click 'Log Entry'.</p>
        </div>
        
        <div id="allEntries" class="tab-content">
            <h2>All Logged Entries</h2>
            <div style="overflow-x: auto;">
                <table id="entriesTable">
                    <thead>
                        <tr>
                            <th>Selection</th>
                            <th>Amount</th>
                            <th>Timestamp</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="entriesTableBody">
                        </tbody>
                </table>
            </div>
            <p class="info-text">Click on any cell (except 'Actions') to edit directly. Changes save automatically.</p>
        </div>
        
        <div id="manageOptions" class="tab-content">
            <h2>Manage Dropdown Options</h2>
            <div id="manageOptionsAlert" class="alert" style="display:none;"></div>

            <div class="form-group">
                <label for="newOptionName" class="form-label">New Option:</label>
                <input type="text" id="newOptionName" class="form-control" placeholder="e.g., Water Intake">
                <button id="addOptionBtn" class="btn btn-success">Add Option</button>
            </div>

            <div style="overflow-x: auto;">
                <table id="optionsTable">
                    <thead>
                        <tr>
                            <th>Option Name</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="optionsTableBody">
                        </tbody>
                </table>
            </div>
            <p class="info-text">Click on an option name to edit it. Hit Enter or click outside to save changes.</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // --- Data Storage Keys ---
            const ENTRIES_KEY = 'simpleLogger_entries';
            const OPTIONS_KEY = 'simpleLogger_options';

            // --- Initial Data Loading ---
            let entries = JSON.parse(localStorage.getItem(ENTRIES_KEY)) || [];
            // Default options if none exist
            let options = JSON.parse(localStorage.getItem(OPTIONS_KEY)) || ['Work Done', 'Daily Task', 'Reading', 'Exercise'];

            // --- DOM Elements ---
            const tabs = document.querySelectorAll('.tab');
            const quickEntryTabContent = document.getElementById('quickEntry');
            const entrySelectionDropdown = document.getElementById('entrySelection');
            const entryDateInput = document.getElementById('entryDate');
            const entryTimeInput = document.getElementById('entryTime');
            const entryAmountInput = document.getElementById('entryAmount');
            const logEntryBtn = document.getElementById('logEntryBtn');
            const quickEntryAlert = document.getElementById('quickEntryAlert');

            const entriesTableBody = document.getElementById('entriesTableBody');

            const newOptionNameInput = document.getElementById('newOptionName');
            const addOptionBtn = document.getElementById('addOptionBtn');
            const optionsTableBody = document.getElementById('optionsTableBody');
            const manageOptionsAlert = document.getElementById('manageOptionsAlert');

            // --- Initial Setup ---
            renderOptionsDropdown(); // Populate dropdown on load
            renderEntriesTable();   // Populate entries table on load
            renderOptionsTable();   // Populate options table on load
            updateDateTimeInputs(); // Set current date/time
            setInterval(updateDateTimeInputs, 1000); // Update every second

            // --- Event Listeners ---

            // Tab Navigation
            tabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    // Remove active class from all tabs and content
                    tabs.forEach(t => t.classList.remove('active'));
                    document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));

                    // Add active class to clicked tab and corresponding content
                    this.classList.add('active');
                    const targetTabId = this.getAttribute('data-tab');
                    document.getElementById(targetTabId).classList.add('active');

                    // Re-render relevant tables when switching tabs to ensure freshness
                    if (targetTabId === 'allEntries') {
                        renderEntriesTable();
                    } else if (targetTabId === 'manageOptions') {
                        renderOptionsTable();
                    }
                });
            });

            // Quick Entry Logging
            logEntryBtn.addEventListener('click', function() {
                const selectedOption = entrySelectionDropdown.value;
                const date = entryDateInput.value;
                const time = entryTimeInput.value;
                const amount = parseFloat(entryAmountInput.value);

                if (!selectedOption) {
                    showAlert(quickEntryAlert, 'Please select an item.', 'danger');
                    return;
                }
                if (isNaN(amount)) {
                    showAlert(quickEntryAlert, 'Amount must be a valid number.', 'danger');
                    return;
                }

                const timestamp = `${date}T${time}`; // ISO format for easy parsing

                const newEntry = {
                    id: Date.now(), // Unique ID for each entry
                    selection: selectedOption,
                    amount: amount,
                    timestamp: timestamp
                };

                entries.push(newEntry);
                saveEntries();
                renderEntriesTable();
                showAlert(quickEntryAlert, 'Entry logged successfully!', 'success');
                // Reset amount to 0 after logging
                entryAmountInput.value = '0';
                updateDateTimeInputs(); // Update date/time to current
            });

            // Manage Options: Add New Option
            addOptionBtn.addEventListener('click', function() {
                const newOption = newOptionNameInput.value.trim();
                if (newOption) {
                    if (!options.includes(newOption)) {
                        options.push(newOption);
                        saveOptions();
                        renderOptionsDropdown();
                        renderOptionsTable();
                        newOptionNameInput.value = '';
                        showAlert(manageOptionsAlert, 'Option added successfully!', 'success');
                    } else {
                        showAlert(manageOptionsAlert, 'Option already exists!', 'warning');
                    }
                } else {
                    showAlert(manageOptionsAlert, 'Option name cannot be empty.', 'danger');
                }
            });

            // --- Data Persistence Functions ---
            function saveEntries() {
                localStorage.setItem(ENTRIES_KEY, JSON.stringify(entries));
            }

            function saveOptions() {
                localStorage.setItem(OPTIONS_KEY, JSON.stringify(options));
            }

            // --- Rendering Functions ---

            function renderOptionsDropdown() {
                entrySelectionDropdown.innerHTML = ''; // Clear existing options
                if (options.length === 0) {
                    const defaultOption = document.createElement('option');
                    defaultOption.value = '';
                    defaultOption.textContent = 'No options available. Add some in "Manage Options".';
                    entrySelectionDropdown.appendChild(defaultOption);
                    entrySelectionDropdown.disabled = true;
                    logEntryBtn.disabled = true;
                    return;
                }
                entrySelectionDropdown.disabled = false;
                logEntryBtn.disabled = false;

                options.forEach(option => {
                    const optionElement = document.createElement('option');
                    optionElement.value = option;
                    optionElement.textContent = option;
                    entrySelectionDropdown.appendChild(optionElement);
                });
            }

            function renderEntriesTable() {
                entriesTableBody.innerHTML = ''; // Clear existing rows

                if (entries.length === 0) {
                    entriesTableBody.innerHTML = '<tr><td colspan="4" style="text-align: center; padding: 20px;">No entries logged yet.</td></tr>';
                    return;
                }

                // Sort entries by timestamp (newest first)
                const sortedEntries = [...entries].sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));

                sortedEntries.forEach(entry => {
                    const row = entriesTableBody.insertRow();
                    row.dataset.id = entry.id; // Store ID for easy lookup

                    // Selection Cell (editable)
                    const selectionCell = row.insertCell();
                    selectionCell.textContent = entry.selection;
                    selectionCell.contentEditable = true;
                    selectionCell.dataset.field = 'selection';
                    selectionCell.addEventListener('blur', handleEntryCellEdit);
                    selectionCell.addEventListener('keydown', handleCellKeydown);


                    // Amount Cell (editable)
                    const amountCell = row.insertCell();
                    amountCell.textContent = entry.amount;
                    amountCell.contentEditable = true;
                    amountCell.dataset.field = 'amount';
                    amountCell.addEventListener('blur', handleEntryCellEdit);
                    amountCell.addEventListener('keydown', handleCellKeydown);


                    // Timestamp Cell (editable) - display formatted, save raw
                    const timestampCell = row.insertCell();
                    timestampCell.textContent = formatTimestamp(entry.timestamp);
                    timestampCell.contentEditable = true;
                    timestampCell.dataset.field = 'timestamp';
                    timestampCell.dataset.rawTimestamp = entry.timestamp; // Store raw value
                    timestampCell.addEventListener('blur', handleEntryCellEdit);
                    timestampCell.addEventListener('keydown', handleCellKeydown);


                    // Actions Cell (Delete Button)
                    const actionsCell = row.insertCell();
                    const deleteButton = document.createElement('button');
                    deleteButton.textContent = 'Delete';
                    deleteButton.classList.add('btn', 'btn-danger');
                    deleteButton.addEventListener('click', function() {
                        deleteEntry(entry.id);
                    });
                    actionsCell.appendChild(deleteButton);
                });
            }

            function renderOptionsTable() {
                optionsTableBody.innerHTML = ''; // Clear existing rows

                if (options.length === 0) {
                    optionsTableBody.innerHTML = '<tr><td colspan="2" style="text-align: center; padding: 20px;">No options configured.</td></tr>';
                    return;
                }

                options.forEach((optionName, index) => {
                    const row = optionsTableBody.insertRow();
                    row.dataset.originalName = optionName; // Store original name for reference

                    // Option Name Cell (editable)
                    const nameCell = row.insertCell();
                    nameCell.textContent = optionName;
                    nameCell.contentEditable = true;
                    nameCell.dataset.field = 'name';
                    nameCell.addEventListener('blur', handleOptionCellEdit);
                    nameCell.addEventListener('keydown', handleCellKeydown);


                    // Actions Cell (Delete Button)
                    const actionsCell = row.insertCell();
                    const deleteButton = document.createElement('button');
                    deleteButton.textContent = 'Delete';
                    deleteButton.classList.add('btn', 'btn-danger');
                    deleteButton.addEventListener('click', function() {
                        deleteOption(optionName);
                    });
                    actionsCell.appendChild(deleteButton);
                });
            }

            // --- Edit Handlers for Tables ---
            function handleEntryCellEdit(event) {
                const cell = event.target;
                const row = cell.closest('tr');
                const entryId = parseInt(row.dataset.id);
                const field = cell.dataset.field;
                let newValue = cell.textContent.trim();

                const entryIndex = entries.findIndex(e => e.id === entryId);
                if (entryIndex === -1) return; // Should not happen

                // Validate and update value
                if (field === 'amount') {
                    newValue = parseFloat(newValue);
                    if (isNaN(newValue)) {
                        showAlert(quickEntryAlert, 'Invalid amount. Reverting to original.', 'danger');
                        cell.textContent = entries[entryIndex].amount; // Revert
                        return;
                    }
                } else if (field === 'timestamp') {
                    // Try to parse the new timestamp
                    const parsedDate = new Date(newValue);
                    if (isNaN(parsedDate.getTime())) {
                        showAlert(quickEntryAlert, 'Invalid date/time format. Reverting to original.', 'danger');
                        cell.textContent = formatTimestamp(entries[entryIndex].timestamp); // Revert
                        return;
                    }
                    newValue = parsedDate.toISOString(); // Store in ISO format
                }

                entries[entryIndex][field] = newValue;
                saveEntries();
                renderEntriesTable(); // Re-render to ensure consistency and re-apply formatting/listeners
                showAlert(quickEntryAlert, 'Entry updated.', 'success');
            }

            function handleOptionCellEdit(event) {
                const cell = event.target;
                const row = cell.closest('tr');
                const oldOptionName = row.dataset.originalName;
                const newOptionName = cell.textContent.trim();

                if (!newOptionName) {
                    showAlert(manageOptionsAlert, 'Option name cannot be empty. Reverting.', 'danger');
                    cell.textContent = oldOptionName; // Revert
                    return;
                }
                
                if (newOptionName === oldOptionName) {
                    // No change, do nothing
                    return;
                }

                if (options.includes(newOptionName)) {
                    showAlert(manageOptionsAlert, 'Option already exists. Reverting.', 'warning');
                    cell.textContent = oldOptionName; // Revert
                    return;
                }

                // Update the option in the array
                const index = options.indexOf(oldOptionName);
                if (index > -1) {
                    options[index] = newOptionName;
                    // Also update any existing entries that used the old option name
                    entries.forEach(entry => {
                        if (entry.selection === oldOptionName) {
                            entry.selection = newOptionName;
                        }
                    });
                    saveOptions();
                    saveEntries(); // Save updated entries
                    renderOptionsDropdown(); // Update dropdown
                    renderOptionsTable();   // Re-render table for freshness
                    renderEntriesTable();   // Re-render entries table too, in case selections changed
                    showAlert(manageOptionsAlert, 'Option updated successfully!', 'success');
                }
            }

            function handleCellKeydown(event) {
                // If Enter is pressed, prevent new line and trigger blur to save
                if (event.key === 'Enter') {
                    event.preventDefault();
                    event.target.blur();
                }
            }


            // --- Delete Functions ---
            function deleteEntry(id) {
                const confirmed = confirm('Are you sure you want to delete this entry?'); // Use browser confirm for simplicity
                if (confirmed) {
                    entries = entries.filter(entry => entry.id !== id);
                    saveEntries();
                    renderEntriesTable();
                    showAlert(quickEntryAlert, 'Entry deleted.', 'success');
                }
            }

            function deleteOption(name) {
                const confirmed = confirm(`Are you sure you want to delete the option "${name}"? This will also clear it from any past entries.`);
                if (confirmed) {
                    options = options.filter(option => option !== name);
                    // Also clear this option from any existing entries
                    entries.forEach(entry => {
                        if (entry.selection === name) {
                            entry.selection = '[DELETED OPTION]'; // Mark as deleted or set to a default
                        }
                    });
                    saveOptions();
                    saveEntries(); // Save updated entries
                    renderOptionsDropdown();
                    renderOptionsTable();
                    renderEntriesTable(); // Re-render entries table to reflect changes
                    showAlert(manageOptionsAlert, 'Option deleted.', 'success');
                }
            }

            // --- Utility Functions ---

            function updateDateTimeInputs() {
                const now = new Date();
                const isoDate = now.toISOString().split('T')[0];
                const isoTime = now.toTimeString().split(' ')[0].substring(0, 5); // HH:MM

                entryDateInput.value = isoDate;
                entryTimeInput.value = isoTime;
            }

            function formatTimestamp(isoString) {
                if (!isoString) return '';
                const date = new Date(isoString);
                // Use Intl.DateTimeFormat for better localization
                const options = {
                    year: 'numeric', month: 'short', day: 'numeric',
                    hour: '2-digit', minute: '2-digit', second: '2-digit',
                    hour12: true // Use 12-hour format with AM/PM
                };
                return date.toLocaleString(undefined, options);
            }

            function showAlert(element, message, type) {
                element.textContent = message;
                element.className = `alert alert-${type}`; // Set class for styling
                element.style.display = 'block';

                setTimeout(() => {
                    element.style.display = 'none';
                }, 3000); // Hide after 3 seconds
            }
        });
    </script>
</body>
</html>
