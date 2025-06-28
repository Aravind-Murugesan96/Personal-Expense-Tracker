# 1. add an expesnse


import csv
def add_expense(expenses):
    date = input("Enter the date in (YYYY-MM-DD) format: ")
    category = input("Enter the category (e.g. Travel, shopping): ")
    amount = float(input("Enter the amount: "))
    description = input("Enter a brief description: ")
    expenses.append({
        "date": date,
        "category": category,
        "amount": amount,
        "description": description
    })
    print("Expense added successfully!")

# 2.view expenses

def view_expenses(expenses):
    
    if not expenses:
        print("No expenses found")
    else:
        for expense in expenses:
            
            if all(key in expense for key in ['date', 'category', 'amount', 'description']):
                print(f"Date: {expense['date']}, Category: {expense['category']}, Amount: {expense['amount']}, Description: {expense['description']}")
            else:
                print(f"Invalid expense record: {expense}")
                
# 3.Set and Track Budget

def set_budget():
    
    budget = float(input("Enter your monthly budget: "))
    return budget


def track_budget(expenses, budget):
    
    total_expenses = sum(expense["amount"] for expense in expenses)
    print(f"Total expenses: {total_expenses}")
    if total_expenses > budget:
        print("Warning: You have exceeded your budget!")
    else:
        print(f"You are within your budget. You have {budget - total_expenses} remaining.")

# 4. Save and load expenses

def save_expenses(expenses, filename="expenses.csv"):
    """Function to save expenses to a file."""
    with open(filename, 'w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(["Date", "Category", "Amount", "Description"])
        for expense in expenses:
            writer.writerow([expense['date'], expense['category'], expense['amount'], expense['description']])
    print("Expenses saved successfully.")
    
def load_expenses(filename="expenses.csv"):
    
    expenses = []
    try:
        with open(filename, 'r') as file:
            reader = csv.DictReader(file)
            for row in reader:
                if all(key in row for key in ['Date', 'Category', 'Amount', 'Description']):                    
                    row['Amount'] = float(row['Amount'])                    
                    expense = {
                        'date': row['Date'],
                        'category': row['Category'],
                        'amount': row['Amount'],
                        'description': row['Description']
                    }
                    expenses.append(expense)
                else:
                    print(f"Skipping invalid expense record: {row}")
    except FileNotFoundError:
        print("No existing expenses found. Starting fresh.")
    return expenses

# 5. Create an interactive menu

def main():
    expenses = load_expenses()
    budget = set_budget()
    while True:
        print("\nPersonal Expense Tracker")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Track Budget")
        print("4. Save Expenses")
        print("5. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            add_expense(expenses)
        elif choice == '2':
            view_expenses(expenses)
        elif choice == '3':
            track_budget(expenses, budget)
        elif choice == '4':
            save_expenses(expenses)
        elif choice == '5':
            save_expenses(expenses)
            print("Exiting...")
            break
        else:
            print("Invalid choice, please select a valid option.")

if __name__ == "__main__":
    main()
