import json
from datetime import datetime

# Function to load transactions from a JSON file
def load_transactions():
    try:
        with open("transactions.json", "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return []

# Function to save transactions to a JSON file
def save_transactions(transactions):
    with open("transactions.json", "w") as file:
        json.dump(transactions, file, indent=4)

# Function to add an expense transaction
def add_expense(transactions, category, amount):
    transactions.append({
        "type": "expense",
        "category": category,
        "amount": amount,
        "date": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    })
    save_transactions(transactions)
    print("Expense added successfully!")

# Function to add an income transaction
def add_income(transactions, amount):
    transactions.append({
        "type": "income",
        "amount": amount,
        "date": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    })
    save_transactions(transactions)
    print("Income added successfully!")

# Function to calculate remaining budget
def calculate_budget(transactions):
    total_income = sum(transaction["amount"] for transaction in transactions if transaction["type"] == "income")
    total_expense = sum(transaction["amount"] for transaction in transactions if transaction["type"] == "expense")
    remaining_budget = total_income - total_expense
    return remaining_budget

# Function to analyze expenses by category
def analyze_expenses(transactions):
    expenses_by_category = {}
    for transaction in transactions:
        if transaction["type"] == "expense":
            category = transaction["category"]
            amount = transaction["amount"]
            expenses_by_category[category] = expenses_by_category.get(category, 0) + amount
    return expenses_by_category

# Main function
def main():
    transactions = load_transactions()
    while True:
        print("\n*** Budget Tracker Menu ***")
        print("1. Add Expense")
        print("2. Add Income")
        print("3. View Remaining Budget")
        print("4. Analyze Expenses by Category")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            category = input("Enter expense category: ")
            amount = float(input("Enter expense amount: "))
            add_expense(transactions, category, amount)
        elif choice == "2":
            amount = float(input("Enter income amount: "))
            add_income(transactions, amount)
        elif choice == "3":
            remaining_budget = calculate_budget(transactions)
            print(f"Remaining budget: {remaining_budget}")
        elif choice == "4":
            expenses_by_category = analyze_expenses(transactions)
            print("Expenses by Category:")
            for category, amount in expenses_by_category.items():
                print(f"{category}: {amount}")
        elif choice == "5":
            print("Exiting program. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")


