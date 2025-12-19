# manju-test
IAM USERS
[
  {
    "user_id": "u1001",
    "name": "Alice",
    "department": "IT",
    "role": "Developer",
    "access": ["GitHub", "Jira"]
  },
  {
    "user_id": "u1002",
    "name": "Bob",
    "department": "Finance",
    "role": "Analyst",
    "access": ["Excel", "FinanceApp"]
  }
]
{
  "Developer": ["GitHub", "Jira"],
  "Analyst": ["Excel", "FinanceApp"],
  "Manager": ["GitHub", "Jira", "FinanceApp"]
}
import json

# Load users
with open("users.json", "r") as f:
    users = json.load(f)

# Load roles
with open("roles.json", "r") as f:
    roles = json.load(f)

def request_access(user_id, requested_access):
    for user in users:
        if user["user_id"] == user_id:
            if requested_access in user["access"]:
                print("Access already exists.")
                return
            
            print(f"Access request submitted for {requested_access}")
            approve = input("Manager approval (yes/no): ")

            if approve.lower() == "yes":
                user["access"].append(requested_access)
                print("Access approved and granted.")
            else:
                print("Access denied.")
            return

    print("User not found.")

def certify_access():
    print("\n--- Access Certification Review ---")
    for user in users:
        print(f"\nUser: {user['name']}")
        print("Current Access:", user["access"])

        review = input("Remove any access? (yes/no): ")
        if review.lower() == "yes":
            remove = input("Enter access name to remove: ")
            if remove in user["access"]:
                user["access"].remove(remove)
                print("Access removed.")
            else:
                print("Access not found.")

# Save updates
def save_changes():
    with open("users.json", "w") as f:
        json.dump(users, f, indent=2)

# --- Menu ---
while True:
    print("\nIAM System Menu")
    print("1. Request Access")
    print("2. Access Certification")
    print("3. Exit")

    choice = input("Choose option: ")

    if choice == "1":
        uid = input("Enter User ID: ")
        access = input("Enter Access Name: ")
        request_access(uid, access)
        save_changes()

    elif choice == "2":
        certify_access()
        save_changes()

    elif choice == "3":
        print("Exiting IAM System.")
        break

    else:
        print("Invalid choice.")


