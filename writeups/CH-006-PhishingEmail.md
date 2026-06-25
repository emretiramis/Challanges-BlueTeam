# Phishing Email

Your email address has been leaked and you receive an email from Paypal in German. Try to analyze the suspicious email.
File location: C:\Users\LetsDefend\Desktop\Files\PhishingChallenge.zip Password: infected

---



## 1. What is the return path of the email?
The main reason why the Return-Path field is critical in email analysis is that attackers can easily type legitimate names like "PayPal" in the From field to trick the user, but they cannot as easily hide the actual return address (Return-Path) that email servers use for undeliverable messages. While these two fields must be compatible and consistent with each other in legitimate e-mails, in phishing attacks, a reliable brand name appears in the From section and an irrelevant or hijacked server address belonging to the attacker usually appears in the Return-Path section.
<img width="1477" height="856" alt="image" src="https://github.com/user-attachments/assets/6b1aac5d-3a55-46b9-a9cd-74d53dbe6997" />




## 2. What is the domain name of the url in this mail?
<img width="1268" height="860" alt="image" src="https://github.com/user-attachments/assets/1abfb43b-9814-49ff-a5cc-86ce5e0f49c4" />



## 3. Is the domain mentioned in the previous question suspicious?
yes, community has reports.
<img width="1713" height="1094" alt="image" src="https://github.com/user-attachments/assets/583e1f2b-cf33-4d6f-ace2-90d7bfedf33d" />



## 4. What is the body SHA-256 of the domain?
13945ecc33afee74ac7f72e1d5bb73050894356c4bf63d02a1a53e76830567f5

<img width="1419" height="955" alt="image" src="https://github.com/user-attachments/assets/dda2362c-3768-4a69-ba61-e1b1ad9e4562" />



## 5. Is this email a phishing email?
Yes
