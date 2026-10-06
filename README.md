# DineDesk - Restaurant Ordering & Table Management System

menu = {
    1: {"name": "Chicken Biryani", "price": 180},
    2: {"name": "Paneer Butter Masala", "price": 160},
    3: {"name": "Masala Dosa", "price": 80},
    4: {"name": "Chicken Fried Rice", "price": 150},
    5: {"name": "Veg Noodles", "price": 120},
    6: {"name": "French Fries", "price": 90},
    7: {"name": "Gulab Jamun", "price": 60},
    8: {"name": "Fresh Lime Juice", "price": 50}
}

orders = []
reservations = []
order_id = 1001


def show_menu():
    print("\n========== DineDesk Menu ==========")
    for item_id, item in menu.items():
        print(f"{item_id}. {item['name']:<25} ₹{item['price']}")
    print("===================================")


def place_order():
    global order_id

    cart = []

    while True:
        show_menu()

        try:
            choice = int(input("\nEnter item number (0 to finish): "))

            if choice == 0:
                break

            if choice not in menu:
                print("Invalid item number.")
                continue

            quantity = int(input("Enter quantity: "))

            if quantity <= 0:
                print("Quantity must be greater than 0.")
                continue

            item = menu[choice]

            cart.append({
                "name": item["name"],
                "price": item["price"],
                "quantity": quantity
            })

            print(f"{item['name']} added to cart.")

        except ValueError:
            print("Please enter a valid number.")

    if not cart:
        print("\nNo items selected.")
        return

    total = sum(
        item["price"] * item["quantity"]
        for item in cart
    )

    print("\n========== ORDER SUMMARY ==========")

    for item in cart:
        amount = item["price"] * item["quantity"]
        print(
            f"{item['name']} x {item['quantity']} "
            f"= ₹{amount}"
        )

    print("-----------------------------------")
    print(f"Total Amount: ₹{total}")

    confirm = input("\nConfirm order? (y/n): ").lower()

    if confirm == "y":
        order = {
            "order_id": order_id,
            "items": cart,
            "total": total,
            "status": "Received"
        }

        orders.append(order)

        print("\nOrder placed successfully!")
        print(f"Your Order ID: {order_id}")

        order_id += 1
    else:
        print("Order cancelled.")


def reserve_table():
    print("\n========== TABLE RESERVATION ==========")

    name = input("Enter customer name: ")
    phone = input("Enter phone number: ")

    try:
        guests = int(input("Number of guests: "))
        table = int(input("Enter table number: "))
    except ValueError:
        print("Invalid input.")
        return

    reservation = {
        "name": name,
        "phone": phone,
        "guests": guests,
        "table": table
    }

    reservations.append(reservation)

    print("\nTable reserved successfully!")
    print(f"Customer : {name}")
    print(f"Table    : {table}")
    print(f"Guests   : {guests}")


def track_order():
    try:
        search_id = int(input("\nEnter Order ID: "))
    except ValueError:
        print("Invalid Order ID.")
        return

    for order in orders:
        if order["order_id"] == search_id:
            print("\n========== ORDER STATUS ==========")
            print(f"Order ID : {order['order_id']}")
            print(f"Status   : {order['status']}")
            print(f"Total    : ₹{order['total']}")
            return

    print("Order not found.")


def kitchen_dashboard():
    print("\n========== KITCHEN DASHBOARD ==========")

    if not orders:
        print("No orders available.")
        return

    for order in orders:
        print(
            f"\nOrder ID: {order['order_id']}"
            f"\nStatus: {order['status']}"
        )

        for item in order["items"]:
            print(
                f"  {item['name']} x {item['quantity']}"
            )

    try:
        order_id_update = int(
            input("\nEnter Order ID to update: ")
        )
    except ValueError:
        print("Invalid Order ID.")
        return

    for order in orders:
        if order["order_id"] == order_id_update:

            print("\n1. Preparing")
            print("2. Ready")
            print("3. Completed")

            choice = input("Select status: ")

            status = {
                "1": "Preparing",
                "2": "Ready",
                "3": "Completed"
            }

            if choice in status:
                order["status"] = status[choice]
                print("Order status updated successfully.")
            else:
                print("Invalid choice.")

            return

    print("Order not found.")


def show_reservations():
    print("\n========== RESERVATIONS ==========")

    if not reservations:
        print("No reservations.")
        return

    for reservation in reservations:
        print(
            f"Customer: {reservation['name']}\n"
            f"Phone: {reservation['phone']}\n"
            f"Guests: {reservation['guests']}\n"
            f"Table: {reservation['table']}\n"
            "--------------------------------"
        )


def main():
    while True:
        print("\n")
        print("====================================")
        print("       🍽️  DineDesk Restaurant")
        print("====================================")
        print("1. View Menu")
        print("2. Place Order")
        print("3. Reserve Table")
        print("4. Track Order")
        print("5. Kitchen Dashboard")
        print("6. View Reservations")
        print("7. Exit")
        print("====================================")

        choice = input("Enter your choice: ")

        if choice == "1":
            show_menu()

        elif choice == "2":
            place_order()

        elif choice == "3":
            reserve_table()

        elif choice == "4":
            track_order()

        elif choice == "5":
            kitchen_dashboard()

        elif choice == "6":
            show_reservations()

        elif choice == "7":
            print("\nThank you for using DineDesk!")
            break

        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
