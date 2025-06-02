<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>When Will You Die?</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
            font-weight: bold;
            /* Consider removing or replacing the background image if it's distracting or not loading */
            /* background-image: url("pro2.jpeg"); */
            /* background-size: cover; */
            /* background-position: center; */
        }
        h1 {
            color:lawngreen;
            text-align: center;
            margin-bottom: 20px;
        }
        fieldset {
            border: 4px solid lawngreen;
            border-radius: 15px;
            padding: 25px;
            width: 100%;
            max-width: 500px;
            background-color:rgba(0, 0, 0, 0.5);;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        /* Using labels for better accessibility and semantics */
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: lawngreen;
        }
        input[type="text"], input[type="number"] {
            height: 40px;
            width: calc(100% - 18px); /* Account for border and padding */
            border: 5px solid lawngreen;
            border-radius: 8px;
            padding: 0 10px;
            margin-bottom: 15px;
            box-sizing: border-box;
            font-size: 16px;
            background-color: transparent;
            font-weight: bold;
            color: lawngreen;
        }
        input:focus{
            border-color: lawngreen;
        }
        button {
            background-color:lawngreen;
            color: black;
            border: none;
            padding: 12px 25px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            width: 300px;
            font-weight: bold;

 }
        button:hover {
            background: transparent;
            color: lawngreen;
        }
        .result-message {
            margin-top: 15px;
            padding: 10px;
            border-radius: 5px;
            font-size: 16px;
            text-align: center;
        }
        .error-message {
            background-color:black; /* Light red */
            color: red; /* Dark red */
            border: 3px solid #c62828;
        }
        .success-message {
            background-color: black; /* Light green */
            color:lawngreen; /* Dark green */
            border: 3px solid lawngreen;
        }
        /* Hide paragraphs initially, show with content */
        #resultOutput, #yearOutput {
            display: none;
        }
    </style>
</head>
<body background="pro222.jpg">
     <h1>WHEN WILL YOU DIE ???</h1>
     <fieldset>
         <label for="userName">*NAME</label>
         <input type="text" id="userName" placeholder="Enter your name" required>

 <label for="yearOfBirth">*YEAR OF BIRTH</label>
         <input type="number" id="yearOfBirth" placeholder="e.g., 1990" required>

 <label for="currentAge">*CURRENT AGE</label>
         <input type="number" id="currentAge" placeholder="e.g., 30" required>


 <p id="resultOutput" class="result-message"></p>
         <p id="yearOutput" class="result-message"></p>
     </fieldset>
     <br>
     <button onclick="calculateOutcome()">PROCESS</button>

 <script>
     function calculateOutcome() {
        // Get input elements
        const nameInput = document.getElementById("userName");
        const yearOfBirthInput = document.getElementById("yearOfBirth");
        const currentAgeInput = document.getElementById("currentAge");

        // Get output elements
        const resultOutputParagraph = document.getElementById("resultOutput");
        const yearOutputParagraph = document.getElementById("yearOutput");

        // Clear previous messages and hide them
        resultOutputParagraph.textContent = "";
        yearOutputParagraph.textContent = "";
        resultOutputParagraph.className = "result-message"; // Reset class
        yearOutputParagraph.className = "result-message";   // Reset class
        resultOutputParagraph.style.display = "none";
        yearOutputParagraph.style.display = "none";

        // Get input values
        const nameValue = nameInput.value.trim();
        const yearOfBirthValue = yearOfBirthInput.value;
        const currentAgeValue = currentAgeInput.value;

        // --- Input Validation ---
        if (nameValue === "") {
            displayMessage(resultOutputParagraph, "Name is required.", true);
            return; // Stop execution
        }

        if (yearOfBirthValue === "") {
            displayMessage(resultOutputParagraph, "Year of Birth is required.", true);
            return;
        }

        if (currentAgeValue === "") {
            displayMessage(resultOutputParagraph, "Current Age is required.", true);
            return;
        }

        // Parse numeric inputs
        const yearOfBirth = parseFloat(yearOfBirthValue);
        const currentAge = parseFloat(currentAgeValue);

        // Validate parsed numbers
        if (isNaN(yearOfBirth)) {
            displayMessage(resultOutputParagraph, "Invalid Year of Birth. Please enter a number.", true);
            return;
        }

        if (isNaN(currentAge)) {
            displayMessage(resultOutputParagraph, "Invalid Current Age. Please enter a number.", true);
            return;
        }

        // Further logical validation
        const currentYear = new Date().getFullYear();

        if (yearOfBirth >= currentYear) {
            displayMessage(resultOutputParagraph, `Year of Birth cannot be in the future or current year (${currentYear}).`, true);
            // Optionally, suggest an age based on a valid birth year if possible, or just error out.
            // For this game, we'll just error out.
            return;
        }

        if (yearOfBirth < 1900) { // Arbitrary lower limit for sensibility
             displayMessage(resultOutputParagraph, "Year of Birth seems too far in the past.", true);
             return;
        }

        if (currentAge <= 0 || currentAge > 120) { // Arbitrary age limits
            displayMessage(resultOutputParagraph, "Current Age is not realistic. Please enter a valid age.", true);
            return;
        }

        // Optional: Check consistency between age and year of birth
        const calculatedAge = currentYear - yearOfBirth;
        if (Math.abs(calculatedAge - currentAge) > 1) { // Allow for 1 year difference (e.g., birthday not yet passed)
            displayMessage(resultOutputParagraph, `Age (${currentAge}) and Year of Birth (${yearOfBirth}) don't seem to match for the current year (${currentYear}). Calculated age is ${calculatedAge}. Please check your inputs.`, true);
            return;
        }

        // --- Calculation (as per original "game" logic) ---
        // Generates a random number of additional years to live (from 1 to 80, for example)
        // The original was 1-100, which could lead to very high ages. Let's make it a bit more "realistic" for the game.
        const randomYearsToAdd = Math.floor(Math.random() * (80 - currentAge > 0 ? Math.min(50, 80 - currentAge) : 20)) + 1; // Max 50 more years, or less if already old. Min 1.
        const predictedDeathAge = currentAge + randomYearsToAdd;
        const predictedDeathYear = yearOfBirth + predictedDeathAge;


        // --- Display Results ---
        displayMessage(resultOutputParagraph, `${nameValue}, based on our *highly scientific* process, you might live to be: ${predictedDeathAge} years old.`, false);
        displayMessage(yearOutputParagraph, `This means you might pass away in the year: ${predictedDeathYear}.`, false);
     }

     /**
      * Helper function to display messages in a paragraph element.
      * @param {HTMLElement} element - The paragraph element to display the message in.
      * @param {string} message - The message text.
      * @param {boolean} isError - True if the message is an error, false otherwise.
      */
     function displayMessage(element, message, isError) {
        element.textContent = message;
        if (isError) {
            element.className = "result-message error-message";
        } else {
            element.className = "result-message success-message";
        }
        element.style.display = "block"; // Make the paragraph visible
     }

     // Optional: Add event listeners for input fields to clear errors on input
     document.getElementById("userName").addEventListener('input', clearAssociatedErrors);
     document.getElementById("yearOfBirth").addEventListener('input', clearAssociatedErrors);
     document.getElementById("currentAge").addEventListener('input', clearAssociatedErrors);

     function clearAssociatedErrors() {
        // A simple way to clear general messages when user starts typing again
        // More sophisticated error handling could target specific error messages
        const resultOutputParagraph = document.getElementById("resultOutput");
        const yearOutputParagraph = document.getElementById("yearOutput");

        if (resultOutputParagraph.style.display !== "none" && resultOutputParagraph.classList.contains("error-message")) {
            resultOutputParagraph.textContent = "";
            resultOutputParagraph.style.display = "none";
            resultOutputParagraph.className = "result-message";
        }
         if (yearOutputParagraph.style.display !== "none" && yearOutputParagraph.classList.contains("error-message")) {
            yearOutputParagraph.textContent = "";
            yearOutputParagraph.style.display = "none";
            yearOutputParagraph.className = "result-message";
        }
     }

    </script>
</body>
</html>
