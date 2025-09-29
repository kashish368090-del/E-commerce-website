# E-commerce-website
Food corner
import streamlit as st
from streamlit_option_menu import option_menu #import sidebar menu option 



# Initialize session state
if "page" not in st.session_state:
    st.session_state.page = "start"
if "cart" not in st.session_state:
    st.session_state.cart = []
if "location" not in st.session_state:
    st.session_state.location = None

menu = {
    # Popular Items
    "Big Mac": {"price": 190.00, "image": "big_mac.jpg"},
    "Spicy McChicken": {"price": 120.00, "image": "spicy_mcchicken.jpg"},
    
    # Drinks & Meals
    "EVM McChicken Meal": {"price": 237.00, "image": "mcchicken_meal.jpg"},
    "Cappuccino": {"price": 200.00, "image": "cappuccino.jpg"},
    "Cold Coffee":{"price":120.00,"image":"cold_coffee.jpg"},
    "Hot Coffee":{"price":100.00,"image":"hot_coffee.jpg"},
    "Tea":{"price":80.00,"image":"tea.jpg"},
    "Green Tea":{"price":60.00,"image":"greentea.jpg"},
    
    "French Fries":{"price":80.00,"image":"french_fries.jpg"},


    
    
    # Veg Burgers
    "Double Tikki Burger": {"price": 149.00, "image": "double_tikki.jpg"},
    "Veg Burger": {"price": 99.00, "image": "veg_burger.jpg"},
    "Veg McPuff": {"price": 80.00, "image": "puff.jpg"},
    "Double Big Tasty Beacon": {"price": 249.00, "image": "big_tasty_beacon.jpg"},
    "Classic Smashed CheeseBurger": {"price": 199.00, "image": "cheese_burger.jpg"},
    
    # Non-Veg Burgers
    "Mc Crispy Chicken Burger": {"price": 219.00, "image": "crispy_chicken.jpg"},
    "Mc Spicy Chicken Burger": {"price": 249.00, "image": "spicy_chicken.jpg"},
    "Peri Peri Chicken Burger": {"price": 299.00, "image": "peri_peri.jpg"},

    # Soft Drinks
    "Coca Cola": {"price": 70.00, "image": "cocacola.jpg"},
    "Sprite": {"price": 70.00, "image": "sprite.jpg"},
    "Fanta": {"price": 70.00, "image": "fanta.jpg"},
    "Sprite Float":{"price":85.00,"image":"sprite_mint.jpg"},
    "Fanta Float":{"price":85,"image":"fanta_mint.jpg"},
    "Mango Float":{"price":145.00,"image":"mango.jpg"},
    "Coke Float":{"price":70.00,"image":"cokefloat.jpg"},

    #Ice Creams
    
    "Cupcake Savvy":{"price":210.00,"image":"scoop.jpg"},
    "Vanilla cone":{"price":250.00,"image":"cone.jpg"},
    "Vanilla Scoop":{"price":300.00,"image":"vannillascoop.jpg"},
    "Butter scotch with choconut":{"price":300.00,"image":"butter.jpg"},
    "Chocolate Volcano":{"price":550.00,"image":"choco.jpg"},


               
}

#displays a title, a description, and three columns each containing an image of payment method
payment_methods = ["Credit/Debit Card", "Paypal", "Cash on Delivery"]

def payment_method_page():
    st.title("Where would you like to Pay?")
    st.write("Select a Payment Method:")
    image1, image2, image3 = st.columns(3) #adds image in 3 columns

    with image1:
        st.image("credit_card.jpg", width=100)
        st.write("Credit/Debit Card")

    with image2:
        st.image("paypal.jpg", width=100)
        st.write("PayPal")

    with image3:
        st.image("cash_on_delivery.jpg", width=100)
        st.write("Cash on Delivery")

#presents a select box for the user to choose a payment method and returns the selected option.
    selected_payment_method = st.selectbox("Payment Method", payment_methods)
    return selected_payment_method


if st.session_state.page == "start": #Checks if the current page is "start"
    st.image("mealbuzz.jpg") #displays an image and a title 
    st.title("ORDER NOW")

    #When "START" button is pressed it changes page to location selection page
    if st.button("START"):
        
        
        st.session_state.page = "location"


 #displays a title and two columns with images and buttons for "Eat In" and "Take Away" options
if st.session_state.page == "location":
    st.title("WHERE WOULD YOU LIKE TO EAT?")
    Location1, Location2 = st.columns(2)

    with Location1:
        st.image("eatin.jpg", width=300)
        if st.button("Eat In", key="eat_in"):
            st.session_state.location = "Eat In"
            st.session_state.page = "menu"






    with Location2:
        st.image("takeaway2.jpg", width=300)
        if st.button("Take Away", key="take_away"):
            st.session_state.location = "Take Away"
            st.session_state.page = "menu"

# Show Menu
if st.session_state.page == "menu":
    st.title(f"Menu - {st.session_state.location}")
   
    

    with st.sidebar:
        st.image("mealbuzz.jpg")
        selected = option_menu(
            "Menu",
            ["Home", "Customize Order", "Order Summary"],
            icons=["house", "gear", "cart"],
            menu_icon="mortarboard",
            default_index=0,
        )

     # Function to display menu items
    def display_items(title, items):
        st.subheader(title)
        cols = st.columns(2)
        for i, item in enumerate(items):
            with cols[i % 2]:
                st.image(menu[item]["image"], width=250, caption=item)
                if st.button(f"Add {item} - INR ₹{menu[item]['price']}", key=f"{title}_{item}"):
                    st.session_state.cart.append((item, menu[item]['price']))


# Display different categories based on the selected menu
    if selected == "Home":
        display_items("Popular Items", ["Big Mac", "Spicy McChicken"])
        display_items("Drinks & Meals", ["EVM McChicken Meal", "Cappuccino","Cold Coffee","Hot Coffee","Tea","Green Tea","French Fries"])
        display_items("Veg Burgers", [
            "Double Tikki Burger",
            "Veg Burger",
            "Double Big Tasty Beacon",
            "Classic Smashed CheeseBurger"
        ])
        display_items("Non-Veg Burgers", [
            "Mc Crispy Chicken Burger",
            "Mc Spicy Chicken Burger",
            "Peri Peri Chicken Burger"
        ])

        display_items("Soft Drinks", [
            "Coca Cola",
            "Sprite",
            "Fanta",
            "Sprite Float",
            "Fanta Float",
            "Mango Float",
            "Coke Float"
        ])

        display_items("Ice Creams", [
            
            "Cupcake Savvy",
            "Vanilla cone",
            "Vanilla Scoop",
            "Butter scotch with choconut",
            "Chocolate Volcano"
        ])

    elif selected == "Customize Order":
        st.subheader("Customize Your Order")
        selected_item = st.selectbox("Choose an item", list(menu.keys()))
        quantity = st.number_input("Quantity", min_value=1, value=1)

        if st.button("Add to Order"):
            st.session_state.cart.append((selected_item, menu[selected_item]["price"] * quantity))
            st.success(f"Added {quantity} x {selected_item} to your order.")

    elif selected == "Order Summary":
        st.subheader("Order Summary")
        if st.session_state.cart:
            total = 0
            for item, price in st.session_state.cart:
                st.write(f"{item} - INR ₹{price:.2f}")
                total += price
            gst = total * 0.05
            st.write(f"Subtotal: INR ₹{total:.2f}")
            st.write(f"GST (5%): INR ₹{gst:.2f}")
            st.subheader(f"Total: INR ₹{total + gst:.2f}")

            st.markdown("---")
            selected_payment_method = payment_method_page()
            st.write(f"You have Selected: *{selected_payment_method}*")

            if selected_payment_method == "Credit/Debit Card":
                st.subheader("Enter Card Details")
                card_number = st.text_input("Card Number")
            elif selected_payment_method == "Paypal":
                st.subheader("Enter Paypal Details")
                paypal_email = st.text_input("Paypal Email")
                paypal_password = st.text_input("Paypal Password", type="password")
            elif selected_payment_method == "Cash on Delivery":
                st.write("Pay with cash when the order is delivered.")
    if st.button("CONFIRM"):
                    st.success("Your order will be processed accordingly.") 

    

    def take_a_seat_or_pickup_at_counter():
        st.title("How would you like to receive your order?")
        options=["Take a Seat","Pick up at a Counter"]
        selected_option=st.selectbox("Select an option",options)
        return selected_option
    selected_option=take_a_seat_or_pickup_at_counter()
    st.write(f"You have Selected : {selected_option}")
    if selected_option=="Take a Seat":
        st.write("We will bring your order to you.Please take a seat and we will let you know when order is ready.")
    elif selected_option=="Pickup at the Counter":
        st.write("Your order will be ready for pickup at the counter.Please come to the counter to collect your order." )
    

    def table_number():
        
        st.title("Table Number")
        table_number=st.text_input("Enter your table Number:")
        
            
    table_number()

#Finalizing the Order
    if st.button("Place Order"):
                st.success("Order placed successfully!")
                st.balloons()
                st.session_state.cart = []
