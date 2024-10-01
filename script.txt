let platesCount = [0, 0, 0, 0];

document.addEventListener('DOMContentLoaded', () => {
    // Load saved plates from local storage
    const savedPlates = JSON.parse(localStorage.getItem('platesCount'));
    if (savedPlates) {
        platesCount = savedPlates;
        updateDisplay();
    }

    // Event listeners for increment and decrement buttons
    document.querySelectorAll('.increment').forEach(button => {
        button.addEventListener('click', function() {
            const personIndex = this.dataset.person - 1;
            platesCount[personIndex]++;
            updateDisplay();
        });
    });

    document.querySelectorAll('.decrement').forEach(button => {
        button.addEventListener('click', function() {
            const personIndex = this.dataset.person - 1;
            if (platesCount[personIndex] > 0) {
                platesCount[personIndex]--;
                updateDisplay();
            }
        });
    });

    // Set the default price
    document.getElementById('price').value = 72;
});

function updateDisplay() {
    platesCount.forEach((count, index) => {
        document.getElementById(`plates${index + 1}`).textContent = count;
        const price = parseFloat(document.getElementById('price').value) || 0;
        document.getElementById(`cost${index + 1}`).textContent = `$${(count * price).toFixed(2)}`;
    });
    localStorage.setItem('platesCount', JSON.stringify(platesCount));
    calculateCost();
}

function calculateCost() {
    const price = parseFloat(document.getElementById('price').value) || 0;
    const totalPlates = platesCount.reduce((sum, count) => sum + count, 0);
    const totalCost = totalPlates * price;
    const costPerPerson = totalPlates > 0 ? (totalCost / 4).toFixed(2) : 0;

    document.getElementById('totalPlates').textContent = totalPlates;
    document.getElementById('totalCost').textContent = totalCost.toFixed(2);
    document.getElementById('costPerPerson').textContent = costPerPerson;
}
