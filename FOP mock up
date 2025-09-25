<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Appraisal Tool</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://rsms.me/inter/inter.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .tab-button.active {
            @apply bg-white text-blue-600 border-b-2 border-blue-600 font-bold;
        }
        .table-row-item {
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            gap: 0.25rem;
            align-items: center;
            padding: 0.25rem;
            border-radius: 0.375rem;
        }
        @media (min-width: 768px) {
            .table-row-item {
                grid-template-columns: repeat(4, minmax(0, 1fr));
                gap: 0.25rem;
            }
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-2 text-sm">

    <!-- Main Container -->
    <div class="bg-white rounded-xl shadow-lg p-4 max-w-4xl w-full">
        <h1 class="text-xl font-bold text-center text-gray-800 mb-4">Car Appraisal Adjustment Manager</h1>

        <!-- Combined Navigation and Button -->
        <div class="flex items-center justify-between border border-gray-300 rounded-md p-2 mb-4">
            <div id="tab-nav" class="flex flex-wrap flex-grow -mb-[1px]">
                <!-- Tabs will be dynamically added here -->
            </div>
            <button id="add-question-btn" class="flex-shrink-0 px-4 py-2 bg-blue-500 text-white font-bold rounded-md shadow-lg hover:bg-blue-600 transition-colors duration-200 text-xs md:text-sm">Add Question</button>
        </div>

        <!-- Tab Content -->
        <div id="tab-content" class="mt-4">
            <!-- Tab content will be dynamically loaded here -->
        </div>
    </div>

    <script>
        // Data structure to hold all appraisal information
        const appraisalData = [
            {
                question: "Accident",
                answers: [
                    { answer: "0", currentAdjustment: "0%", color: "gray" },
                    { answer: "1", currentAdjustment: "-5%", color: "red" },
                    { answer: "2+", currentAdjustment: "-7.50%", color: "red" }
                ],
                rules: []
            },
            {
                question: "Condition",
                answers: [
                    { answer: "Fair", currentAdjustment: "-$1,200", color: "red" },
                    { answer: "Good", currentAdjustment: "$0", color: "gray" },
                    { answer: "Very Good", currentAdjustment: "$500", color: "green" }
                ],
                rules: []
            },
            {
                question: "Mechanical Defects",
                answers: [
                    { answer: "0 Lights", currentAdjustment: "$0", color: "gray" },
                    { answer: "1 Light", currentAdjustment: "-$500", color: "red" },
                    { answer: "2 Lights", currentAdjustment: "-$1,000", color: "red" },
                    { answer: "3 Lights", currentAdjustment: "-$1,500", color: "red" },
                    { answer: "4+ Lights", currentAdjustment: "-$2,000", color: "red" }
                ],
                rules: []
            },
            {
                question: "AC Working",
                answers: [
                    { answer: "Yes", currentAdjustment: "$0", color: "gray" },
                    { answer: "No", currentAdjustment: "-$1,250", color: "red" }
                ],
                rules: []
            },
            {
                question: "Dents/Dings",
                answers: [
                    { answer: "0", currentAdjustment: "$0", color: "gray" },
                    { answer: "1-2", currentAdjustment: "-$200", color: "red" },
                    { answer: "3-5", currentAdjustment: "-$400", color: "red" },
                    { answer: "6+", currentAdjustment: "-$2,000", color: "red" }
                ],
                rules: []
            },
            {
                question: "Odors",
                answers: [
                    { answer: "No", currentAdjustment: "$0", color: "gray" },
                    { answer: "Yes", currentAdjustment: "-$1,000", color: "red" }
                ],
                rules: []
            },
            {
                question: "Interior Damage",
                answers: [
                    { answer: "Dashboard/interior panels", currentAdjustment: "-$500", color: "red" },
                    { answer: "Rips/tears in seats", currentAdjustment: "-$250", color: "red" },
                    { answer: "Stains", currentAdjustment: "-$250", color: "red" },
                    { answer: "Infotainment - Tier 1", currentAdjustment: "-$1,500", color: "red" },
                    { answer: "Infotainment - Tier 2", currentAdjustment: "-$3,000", color: "red" }
                ],
                rules: []
            },
            {
                question: "Salvage/Flood/Rebuilt",
                answers: [
                    { answer: "No", currentAdjustment: "$0", color: "gray" },
                    { answer: "Yes", currentAdjustment: "-$999", color: "red" }
                ],
                rules: []
            },
            {
                question: "Tires",
                answers: [
                    { answer: "Less than 18 months old", currentAdjustment: "$0", color: "gray" },
                    { answer: "18+ months - Tier 1", currentAdjustment: "-$1,000", color: "red" },
                    { answer: "18+ months - Tier 2", currentAdjustment: "-$1,750", color: "red" }
                ],
                rules: []
            },
            {
                question: "Keys",
                answers: [
                    { answer: "2+ keys", currentAdjustment: "$0", color: "gray" },
                    { answer: "<2 keys - Tier 1", currentAdjustment: "-$350", color: "red" },
                    { answer: "<2 keys - Tier 2", currentAdjustment: "-$500", color: "red" }
                ],
                rules: []
            }
        ];

        let activeTab = appraisalData[0].question;

        // Function to render the tab content
        const renderContent = () => {
            const tabContentDiv = document.getElementById('tab-content');
            tabContentDiv.innerHTML = ''; // Clear previous content
            const activeData = appraisalData.find(item => item.question === activeTab);

            // Display Question
            const questionTitle = document.createElement('h2');
            questionTitle.textContent = activeData.question;
            questionTitle.className = 'text-lg font-bold text-gray-800 mb-2';
            tabContentDiv.appendChild(questionTitle);

            // Display Standard Adjustments Table
            const standardTable = document.createElement('div');
            standardTable.className = 'bg-white rounded-lg shadow p-3 mb-4';
            standardTable.innerHTML = `
                <h3 class="font-bold text-gray-700 mb-2">Standard Adjustments</h3>
                <div class="table-row-item font-bold text-gray-600 border-b border-gray-300 pb-1 mb-1 text-xs md:text-sm">
                    <div class="hidden md:block">Answer</div>
                    <div class="text-center">Current Value</div>
                    <div class="text-center">New Value</div>
                </div>
            `;
            activeData.answers.forEach(answer => {
                const colorClass = answer.color === 'red' ? 'bg-red-100 text-red-800' : (answer.color === 'green' ? 'bg-green-100 text-green-800' : 'bg-gray-100 text-gray-800');
                standardTable.innerHTML += `
                    <div class="table-row-item items-center p-1">
                        <div class="text-gray-600 hidden md:block">${answer.answer}</div>
                        <div class="text-center font-semibold rounded-md p-1 ${colorClass}">${answer.currentAdjustment}</div>
                        <div>
                            <input type="text" placeholder="New value" class="w-full px-2 py-1 text-xs text-gray-700 placeholder-gray-400 bg-gray-200 border border-transparent rounded-md focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-transparent">
                        </div>
                    </div>
                `;
            });
            tabContentDiv.appendChild(standardTable);

            // Display Rules
            const rulesSection = document.createElement('div');
            rulesSection.className = 'bg-white rounded-lg shadow p-3 mb-4';
            rulesSection.innerHTML = `<h3 class="font-bold text-gray-700 mb-2">Custom Rules</h3>`;
            if (activeData.rules.length > 0) {
                activeData.rules.forEach((rule, index) => {
                    rulesSection.innerHTML += `
                        <div class="flex items-center justify-between bg-gray-50 rounded-md p-2 mb-2">
                            <p class="font-semibold">If <span class="text-blue-600">${rule.variable}</span> is <span class="text-blue-600">${rule.condition}</span>, then adjustment is <span class="text-green-600">${rule.value}</span></p>
                            <button data-index="${index}" class="remove-rule-btn ml-2 px-2 py-1 bg-red-500 text-white rounded-full text-xs hover:bg-red-600 transition-colors duration-200">Remove</button>
                        </div>
                    `;
                });
            } else {
                rulesSection.innerHTML += `<p class="text-gray-500 text-sm">No custom rules have been added yet.</p>`;
            }
            tabContentDiv.appendChild(rulesSection);

            // Add Rule Form
            const addRuleForm = document.createElement('div');
            addRuleForm.className = 'bg-white rounded-lg shadow p-3';
            addRuleForm.innerHTML = `
                <h3 class="font-bold text-gray-700 mb-2">Add New Rule</h3>
                <div class="flex flex-col md:flex-row gap-2 items-end">
                    <input type="text" id="rule-variable" placeholder="Variable (e.g., brand)" class="w-full md:w-1/3 px-2 py-1 text-xs text-gray-700 placeholder-gray-400 bg-gray-200 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-500">
                    <select id="rule-condition" class="w-full md:w-1/3 px-2 py-1 text-xs text-gray-700 bg-gray-200 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-500">
                        <option value="">Select an answer...</option>
                        ${activeData.answers.map(a => `<option value="${a.answer}">${a.answer}</option>`).join('')}
                    </select>
                    <input type="text" id="rule-value" placeholder="New adjustment value" class="w-full md:w-1/3 px-2 py-1 text-xs text-gray-700 placeholder-gray-400 bg-gray-200 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-500">
                    <button id="add-rule-btn" class="w-full md:w-1/3 px-4 py-2 bg-blue-500 text-white font-bold rounded-md shadow-lg hover:bg-blue-600 transition-colors duration-200 text-sm">Add Rule</button>
                </div>
            `;
            tabContentDiv.appendChild(addRuleForm);

            // Add event listener to the Add Rule button
            document.getElementById('add-rule-btn').addEventListener('click', () => {
                const variable = document.getElementById('rule-variable').value;
                const condition = document.getElementById('rule-condition').value;
                const value = document.getElementById('rule-value').value;

                if (variable && condition && value) {
                    activeData.rules.push({ variable, condition, value });
                    // Clear inputs and re-render
                    document.getElementById('rule-variable').value = '';
                    document.getElementById('rule-condition').value = '';
                    document.getElementById('rule-value').value = '';
                    renderContent();
                }
            });

            // Add event listener for removing rules
            document.querySelectorAll('.remove-rule-btn').forEach(button => {
                button.addEventListener('click', (event) => {
                    const ruleIndex = event.target.dataset.index;
                    activeData.rules.splice(ruleIndex, 1);
                    renderContent();
                });
            });
        };

        // Function to render the tab navigation
        const renderTabs = () => {
            const tabNavDiv = document.getElementById('tab-nav');
            tabNavDiv.innerHTML = '';
            appraisalData.forEach(item => {
                const button = document.createElement('button');
                button.textContent = item.question;
                button.className = `tab-button px-2 py-2 md:px-4 text-xs md:text-sm text-gray-500 hover:text-blue-600 hover:bg-gray-50 transition-colors duration-200 rounded-t-md ${item.question === activeTab ? 'active' : ''} font-bold`;
                button.addEventListener('click', () => {
                    activeTab = item.question;
                    document.querySelectorAll('.tab-button').forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    renderContent();
                });
                tabNavDiv.appendChild(button);
            });
        };

        // Initial render
        document.addEventListener('DOMContentLoaded', () => {
            renderTabs();
            renderContent();
        });

        // Add event listener for the new "Add Question" button
        document.getElementById('add-question-btn').addEventListener('click', () => {
            const newQuestionName = prompt('Please enter the name for the new question:');
            if (newQuestionName) {
                const newQuestion = {
                    question: newQuestionName,
                    answers: [],
                    rules: []
                };
                appraisalData.push(newQuestion);
                activeTab = newQuestionName;
                renderTabs();
                renderContent();
            }
        });
    </script>
</body>
</html>
