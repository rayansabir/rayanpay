<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>رايان باي</title>
  <style>
    body {
      background-color: #121212;
      color: #f0f0f0;
      font-family: 'Tahoma', sans-serif;
      padding: 20px;
      max-width: 500px;
      margin: auto;
    }

    h1, h3, label {
      color: #f0f0f0;
    }

    input, select, button {
      width: 100%;
      padding: 10px;
      margin-top: 8px;
      margin-bottom: 20px;
      border-radius: 6px;
      border: none;
      font-size: 16px;
    }

    input, select {
      background-color: #1e1e1e;
      color: white;
    }

    button {
      background-color: #00c853;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background-color: #00b342;
    }

    ::placeholder {
      color: #999;
    }
  </style>
</head>
<body>

  <h1>رايان باي</h1>

  <h3>طريقة الدفع:</h3>
  <select id="paymentMethod" onchange="updatePaymentInfo()">
    <option disabled selected>اختر طريقة الدفع</option>
    <option value="ZainCash">زين كاش</option>
    <option value="Fib">فيب بنك</option>
    <option value="FastPay">فاست باي</option>
  </select>

  <h3>تريد الاستلام عن طريق:</h3>
  <select id="receiveMethod">
    <option disabled selected>اختر طريقة الاستلام</option>
    <option value="ZainCash">زين كاش</option>
    <option value="Fib">فيب بنك</option>
    <option value="FastPay">فاست باي</option>
  </select>

  <h3>كم المبلغ الذي تريد ارساله؟</h3>
  <input type="text" id="amount" placeholder="ادخل المبلغ" oninput="formatAmount()" />

  <h3>يجب أن تدفع (مع ٢٪):</h3>
  <input type="text" id="total" readonly />

  <h3>ارسل المبلغ إلى الرقم: <span id="phoneNumber">07504340858</span></h3>

  <label for="transactionId">اكتب رقم المعاملة:</label>
  <input type="text" id="transactionId" placeholder="ادخل رقم المعاملة هنا" />

  <h3>أريد الاستلام على هذا الرقم:</h3>
  <input type="text" id="userPhone" placeholder="ادخل رقم هاتفك" />

  <button onclick="sendEmail()">إرسال</button>

  <script>
    function formatAmount() {
      const amountInput = document.getElementById('amount');
      let raw = amountInput.value.replace(/,/g, '').replace(/[^\d]/g, '');

      if (raw) {
        const formatted = new Intl.NumberFormat('en-US').format(raw) + ' IQD';
        amountInput.value = formatted;

        const amountNum = parseFloat(raw);
        const totalWithFee = amountNum + (amountNum * 0.02);

        document.getElementById('total').value = totalWithFee.toLocaleString('en-US', {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2
        }) + ' IQD';
      } else {
        document.getElementById('total').value = '';
      }
    }

    function updatePaymentInfo() {
      const method = document.getElementById('paymentMethod').value;

      const phoneMap = {
        "ZainCash": "07504340858",
        "Fib": "07801234567",
        "FastPay": "07709876543"
      };

      const phone = phoneMap[method] || "_____";
      document.getElementById('phoneNumber').innerText = phone;
    }

    function sendEmail() {
      const amountRaw = document.getElementById('amount').value.replace(/[^0-9]/g, '');
      const total = document.getElementById('total').value;
      const method = document.getElementById('paymentMethod').value;
      const receive = document.getElementById('receiveMethod').value;
      const phone = document.getElementById('phoneNumber').innerText;
      const transactionId = document.getElementById('transactionId').value;
      const userPhone = document.getElementById('userPhone').value;

      const body = `طريقة الدفع: ${method}
طريقة الاستلام: ${receive}
المبلغ المرسل: ${amountRaw} IQD
الإجمالي مع 2%: ${total}
ارسل إلى الرقم: ${phone}
رقم المعاملة: ${transactionId}
يريد الاستلام على الرقم: ${userPhone}`;

      window.location.href = `mailto:rayandoski03@gmail.com?subject=طلب تحويل من رايان باي&body=${encodeURIComponent(body)}`;

      // Show confirmation
      const confirmation = document.createElement('div');
      confirmation.innerText = '✅ تم إرسال طلبك بنجاح!';
      confirmation.style.marginTop = '20px';
      confirmation.style.padding = '15px';
      confirmation.style.background = '#1e1e1e';
      confirmation.style.color = 'lightgreen';
      confirmation.style.border = '1px solid green';
      confirmation.style.borderRadius = '8px';
      confirmation.style.fontSize = '18px';

      document.body.appendChild(confirmation);
    }
  </script>

</body>
</html>
