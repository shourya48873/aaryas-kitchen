MENU = {
    1: {"name": "Burger", "price": 5.99},
    2: {"name": "Pizza", "price": 8.49},
    3: {"name": "Pasta", "pErice": 7.25},
    4: {"name": "Salad", "price": 4.50}
}

def home():
    return render_template('menu.html', menu=MENU)


def add_to_cart(item_id):
    cart = session.get('cart', {})
    cart[str(item_id)] = cart.get(str(item_id), 0) + 1
    session['cart'] = cart
    return redirect(url_for('home'))


def cart():
    cart = session.get('cart', {})
    cart_items = []
    total = 0
    for item_id, quantity in cart.items():
        item = MENU[int(item_id)]
        subtotal = item['price'] * quantity
        cart_items.append({
            'id': item_id,
            'name': item['name'],
            'price': item['price'],
            'quantity': quantity,
            'subtotal': subtotal
        })
        total += subtotal
    return render_template('cart.html', cart_items=cart_items, total=total)


def checkout():
    session.pop('cart', None)
    return render_template('checkout.html')


