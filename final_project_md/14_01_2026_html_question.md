d-flex flex-column: 將整個網頁設為一個垂直的 Flex 容器。

h-100 / min-vh-100: 確保 body 攞足全螢幕高度。如果冇呢個，body 只會跟內容高度縮細，Footer 就唔會痴底。

flex-shrink-0: 呢個放喺 <main> 係為咗防止內容過多時，Footer 反而把內容「夾細」。
edit in base.html `  <body class="d-flex flex-column h-100">` and `    <main class="flex-shrink-0">{% block content %} {% endblock %}</main>`
