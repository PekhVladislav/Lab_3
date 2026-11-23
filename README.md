# Практична робота № 3
# Мінімагазин у консолі
# Виконав: Пех Владислав, група 21-КБ

# Каталог товарів
products = {
    1: {"name": "Ноутбук", "price": 25000.00, "stock": 5},
    2: {"name": "Мишка", "price": 450.50, "stock": 15},
    3: {"name": "Клавіатура", "price": 1200.00, "stock": 10},
    4: {"name": "Навушники", "price": 850.75, "stock": 8}
}

cart = {}

ADMIN_LOGIN = "admin"
ADMIN_PASSWORD = "1234"


# Форматування ціни
format_price = lambda price: f"{price:.2f}грн"


# Показ каталогу
def show_catalog():
    print("\n========== КАТАЛОГ ТОВАРІВ ==========")
    for product_id, product in products.items():
        print(
            f"{product_id}. {product['name']} | "
            f"Ціна: {format_price(product['price'])} | "
            f"Залишок: {product['stock']} шт."
        )


# Показ кошика
def show_cart():
    print("\n=============== КОШИК ===============")
    if not cart:
        print("Кошик порожній.")
        return

    total = 0
    for product_id, quantity in cart.items():
        product = products[product_id]
        cost = product["price"] * quantity
        total += cost
        print(
            f"{product['name']} x {quantity} = "
            f"{format_price(cost)}"
        )

    print("-------------------------------------")
    print(f"До сплати: {format_price(total)}")


# Додавання товару в кошик
def add_to_cart():
    show_catalog()

    try:
        product_id = int(input("\nВведіть номер товару: "))
        quantity = int(input("Введіть кількість: "))

        if product_id not in products:
            print("Такого товару не існує.")
            return

        if quantity <= 0:
            print("Кількість повинна бути більшою за нуль.")
            return

        available = products[product_id]["stock"] - cart.get(product_id, 0)

        if quantity > available:
            print(f"Недостатньо товару. Доступно: {available} шт.")
            return

        cart[product_id] = cart.get(product_id, 0) + quantity
        print("Товар успішно додано до кошика!")

    except ValueError:
        print("Помилка! Введіть ціле число.")


# Видалення товару з кошика
def remove_from_cart():
    if not cart:
        print("\nКошик порожній.")
        return

    show_cart()

    try:
        product_id = int(input("\nВведіть номер товару для видалення: "))

        if product_id not in cart:
            print("Такого товару немає в кошику.")
