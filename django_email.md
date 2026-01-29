Sending Email , connect Google

```
https://docs.djangoproject.com/en/6.0/topics/email/
```

contacts/view.py

```python
def contacts(request):
  if request=="POST":
    listing = request.POST['listing']
    listing_id = request.POST['listing_id']
    name = request.POST['name']
    email = request.POST['email']
    phone = request.POST['phone']
    message = request.POST['message']
    user_id = request.POST['user_id']
    if request.user.is_authenticated:
      has_contacted = Contact.objects.all().filter(listing_id=listing_id,user_id=user_id)
      if has_contacted:
        messages.error(request,'You have already made an inquiry for this listing')
        return redirect('listing:listing', listing_id=listing_id)
      contact = Contact(listing=listing,listing_id=listing_id,name=name,email=email,phone=phone,message=message,user_id=user_id)
      contact.save()
      # ==== send mail function
      send_mail(
          'Clinic Inquiry', # title
          'There has been abn inquiry for ' + listing + # content
          ' . Sign into the admin panel for more info', # content
          'freetousegpt@gmail.com', # from email
          [listing.doctor.email], # to email , need array / list
          fail_silently=False
      )
      # =====
      messages.success(request,'Your request has been submitted, a realtor will get back to you soon')
      return redirect('listings:listing', listing_id=listing_id)
  return render(request,'listings/listings.html')
```

Move to config/settings.py

```python
EMAIL_BACKEND = "django.core.mail.backends.smtp.EmailBackup"
EMAIL_HOST = "smtp.gmail.com"
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = os.getenv('EMAIL_USER')
EMAIL_HOST_PASSWORD = os.getenv('EMAIL_PASS')
```

Move to youtube.com,search

```
How To Set up Gmail SMTP Server
https://www.youtube.com/watch?v=ZfEK3WP73eY
```

```
https://myaccount.google.com/
```

1. 安全性與登入
2. 兩步驗證碼
3. 應用程式密碼
4. 第二步要全部tick晒
5. 啟用兩步驗證碼
6. search **應用程式密碼**

Retail email:

```
https://resend.com
```

```
https://try.mailgun.com/api-1/?utm_source=google&utm_medium=cpc&utm_campaign=APAC%20%7C%20EN%20%7C%20Brand&utm_id=21209483929&utm_content=161813825272&utm_term=mailgun&gad_source=1&gad_campaignid=21209483929&gbraid=0AAAAAofVncfJLVo3AnkzwHBEqOeqKkYtv&gclid=Cj0KCQiApfjKBhC0ARIsAMiR_Iu7YIqcIglpCW8A4shgvZQihzTf4COgSniPk1ofEISKaWpK5N445WAaAuZ3EALw_wcB
```

1.去localhost:8000/admin,搵個valid/real既email
2.Django 準備發送重設密碼的郵件時，它會讀取一個預設的郵件模板
該模板中有一行代碼試圖生成重設連結： {% url 'password_reset_confirm' %} ...
但因為你用了 app_name = 'users'，
Django 找不到 password_reset_confirm，
它現在的名字叫做 users:password_reset_confirm
