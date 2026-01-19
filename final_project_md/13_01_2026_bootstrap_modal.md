#### Bootstrap Modal (模態視窗)

```
<div class="pip-trigger" data-toggle="modal" data-target="#contactModal" style="cursor: pointer;">
  <i class="fa-regular fa-comment" style="font-size: 24px; color: #007bff;"></i>
</div>

<div class="modal fade" id="contactModal" tabindex="-1" role="dialog" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered" role="document">
    <div class="modal-content" style="border-radius: 15px;">

      <div class="modal-body">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>

        <div class="message-shop text-center">
          <h4 class="title">聯絡我們</h4>
          <p class="subtitle text-muted" style="font-size: 0.9rem;">
            謝謝您的主動聯絡，請留下要諮詢的問題，我們會用以下資訊進行回覆。
          </p>
        </div>

        <form action="{% url 'pages:contact' %}" method="POST" id="messageForm">
          {% csrf_token %}

          <div class="form-group">
            <input class="form-control" name="sender_email" placeholder="輸入你的電子信箱" type="email" required>
          </div>

          <div class="form-group">
            <textarea class="form-control" name="message_text" rows="4" placeholder="輸入你的訊息..." required></textarea>
          </div>

          <div class="text-right">
            <button type="submit" class="btn btn-primary btn-block rounded">發送</button>
          </div>
        </form>
      </div>

    </div>
  </div>
</div>
```
