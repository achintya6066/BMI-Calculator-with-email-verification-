# BMI-Calculator-with-email-verification-
import smtplib
import random
from email.message import EmailMessage



def calculate_bmi(weight, height): 
    bmi = weight / (height ** 2) 
     
    if bmi < 18.5: 
        category = "Underweight"
    elif bmi < 25: 
        category = "Normal weight"
    elif bmi < 30: 
        category = "Overweight"
    else: 
        category = "Obese" 
     
    return bmi, category


def diet_plan(category):
    if category == "Underweight":
        return (" Underweight Diet Plan:\n"
            "- Eat calorie-dense foods\n"
            "- Add healthy fats\n"
            "- Have 5–6 meals a day\n"
            "- Include milkshakes & smoothies")

    elif category == "Overweight":
        return ("️ Overweight Diet Plan:\n"
            "- Avoid sugary and fried foods\n"
            "- Eat more vegetables & oats\n"
            "- Drink plenty of water\n"
            "- Avoid late-night eating")
    
    elif category == "Obese":
        return (" Obese Diet Plan:\n"
            "- No junk or sugary drinks\n"
            "- Practice portion control\n"
            "- Prefer salads & fruits\n"
            "- Walk at least 45 minutes daily")

    else:
        return " Normal range! Maintain a balanced diet."
    
def send_otp(receiver_email):
    otp = str(random.randint(100000, 999999))

    msg = EmailMessage()
    msg["Subject"] = "Your Gym OTP Verification"
    msg["From"] = SENDER_EMAIL         
    msg["To"] = receiver_email

    msg.set_content(
        f"Your OTP code is: {otp}\n\n"
        "Use this to verify your gym plan request.\n\n"
        "-----\n"
        "🔥 GuiltFreeFitt Special Offer! 🔥\n"
        "Get 20% OFF on GuiltFreeFitt Membership!\n"
        "Use Promo Code: GUILTFREE\n"
        "-----\n"
        "Stay fit, stay happy ❤️\n"
        )

    try:
        with smtplib.SMTP("smtp.gmail.com", 587) as server:
            server.starttls()
            server.login(SENDER_EMAIL, APP_PASSWORD)   
            server.send_message(msg)

        print("\n OTP sent successfully!")
        return otp

    except Exception as e:
        print("\n Failed to send OTP.")
        print("Error:", e)
        return None
SENDER_EMAIL = "guiltfreefitt@gmail.com"    
APP_PASSWORD = "twcn kshu nyie xwgu"  

weight = float(input("Enter weight (kg): ")) 
height = float(input("Enter height (m): ")) 
     
bmi, category = calculate_bmi(weight, height) 
diet = diet_plan(category)

print(f"\nBMI: {bmi:}") 
print(f"Category: {category}")
print("\nRecommended Diet Plan:")
print(diet)
if category in ("Overweight", "Obese"):
    choice = input("\nWould you like to join our gym and receive plan details? (yes/no): ").lower()

    if choice == "yes":
        email = input("Enter your Gmail address to receive OTP: ")

        sent_otp = send_otp(email)

        if sent_otp:
            user_otp = input("Enter the OTP sent to your email: ")

            if user_otp == sent_otp:
                print("\n OTP verified! We will contact you with gym plans soon.")
            else:
                print("\n Incorrect OTP. Verification failed.")

    else:
        print("\nAlright! Stay healthy and follow your diet plan ")

else:
    print("\nYou are in the normal range, keep maintaining your health! ")
    
