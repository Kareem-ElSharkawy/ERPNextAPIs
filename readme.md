# ERPNext API Requests Extracted

> Source file: `Pasted text(197).txt`  
> Integration: ERPNext / Frappe REST API  
> Base URL placeholder: `{{base_url}}`  
> Example: `https://erp.example.com`

---

## 1. Authentication

### 1.1 Password Login

Used when config has `erp_username` + `erp_password`.

```http
POST {{base_url}}/api/method/login
Content-Type: application/json
Accept: application/json
```

**Body**

```json
{
  "usr": "{{erp_username}}",
  "pwd": "{{erp_password}}"
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/method/login" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -c cookies.txt \
  -b cookies.txt \
  --data '{
    "usr": "{{erp_username}}",
    "pwd": "{{erp_password}}"
  }'
```

---

### 1.2 Token Authentication Header

Used when config has `erp_api_key` + `erp_api_secret`.

```http
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
Accept: application/json
```

---

## 2. Test Connection

### Get Logged User

```http
GET {{base_url}}/api/method/frappe.auth.get_logged_user
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -X GET "{{base_url}}/api/method/frappe.auth.get_logged_user" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}"
```

---

## 3. Customer Requests

### 3.1 Get Customer By ERP ID / Name

```http
GET {{base_url}}/api/resource/Customer/{{customer_name}}
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -X GET "{{base_url}}/api/resource/Customer/{{customer_name}}" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}"
```

---

### 3.2 Search Customer By VAT / Tax ID

```http
GET {{base_url}}/api/resource/Customer?filters={{filters}}&fields={{fields}}&limit_page_length=1
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Query Example**

```text
filters=[["tax_id","=","{{vat_number}}"]]
fields=["name","customer_name","tax_id"]
limit_page_length=1
```

**cURL**

```bash
curl -G "{{base_url}}/api/resource/Customer" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data-urlencode 'filters=[["tax_id","=","{{vat_number}}"]]' \
  --data-urlencode 'fields=["name","customer_name","tax_id"]' \
  --data-urlencode "limit_page_length=1"
```

---

### 3.3 Search Customer By Dynamic Field

Supported field mapping from code:

| Local Field | ERPNext Field |
|---|---|
| `vat_number` | `tax_id` |
| `tax_number` | `tax_id` |
| `email` | `email_id` |
| `phone` | `mobile_no` |
| `name` | `customer_name` |

```http
GET {{base_url}}/api/resource/Customer?filters={{filters}}&fields={{fields}}&limit_page_length=1
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -G "{{base_url}}/api/resource/Customer" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data-urlencode 'filters=[["{{erpnext_field}}","=","{{value}}"]]' \
  --data-urlencode 'fields=["name","customer_name","tax_id"]' \
  --data-urlencode "limit_page_length=1"
```

---

### 3.4 Get First Non-Group Customer Group

Used as fallback when `default_customer_group` is empty.

```http
GET {{base_url}}/api/resource/Customer Group?filters={{filters}}&fields={{fields}}&limit_page_length=1
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -G "{{base_url}}/api/resource/Customer Group" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data-urlencode 'filters=[["is_group","=",0]]' \
  --data-urlencode 'fields=["name"]' \
  --data-urlencode "limit_page_length=1"
```

---

### 3.5 Get First Non-Group Territory

Used as fallback when `default_territory` is empty.

```http
GET {{base_url}}/api/resource/Territory?filters={{filters}}&fields={{fields}}&limit_page_length=1
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -G "{{base_url}}/api/resource/Territory" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data-urlencode 'filters=[["is_group","=",0]]' \
  --data-urlencode 'fields=["name"]' \
  --data-urlencode "limit_page_length=1"
```

---

### 3.6 Create Customer

```http
POST {{base_url}}/api/resource/Customer
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "customer_name": "{{customer_name}}",
  "customer_type": "Company",
  "customer_group": "{{customer_group}}",
  "territory": "{{territory}}",
  "tax_id": "{{vat_number}}",
  "mobile_no": "{{contact_mobile}}",
  "email_id": "{{contact_email}}"
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/resource/Customer" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "customer_name": "{{customer_name}}",
    "customer_type": "Company",
    "customer_group": "{{customer_group}}",
    "territory": "{{territory}}",
    "tax_id": "{{vat_number}}",
    "mobile_no": "{{contact_mobile}}",
    "email_id": "{{contact_email}}"
  }'
```

---

## 4. Sales Invoice Requests

### 4.1 Create Sales Invoice

```http
POST {{base_url}}/api/resource/Sales Invoice
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "doctype": "Sales Invoice",
  "customer": "{{customer_erp_id}}",
  "posting_date": "{{posting_date}}",
  "due_date": "{{due_date}}",
  "currency": "{{currency}}",
  "company": "{{company}}",
  "title": "{{invoice_number}}",
  "remarks": "{{notes}}",
  "taxes_and_charges": "{{default_tax_template}}",
  "items": [
    {
      "item_code": "{{item_code}}",
      "description": "{{description}}",
      "qty": 1,
      "rate": 100,
      "income_account": "{{default_income_account}}",
      "cost_center": "{{default_cost_center}}"
    }
  ]
}
```

**Credit Note / Return**

When `mov_type = credit_note`, the request still uses `Sales Invoice`, but adds:

```json
{
  "is_return": 1
}
```

And item quantities become negative:

```json
{
  "qty": -1,
  "rate": 100
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/resource/Sales Invoice" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "doctype": "Sales Invoice",
    "customer": "{{customer_erp_id}}",
    "posting_date": "{{posting_date}}",
    "due_date": "{{due_date}}",
    "currency": "{{currency}}",
    "company": "{{company}}",
    "title": "{{invoice_number}}",
    "remarks": "{{notes}}",
    "taxes_and_charges": "{{default_tax_template}}",
    "items": [
      {
        "item_code": "{{item_code}}",
        "description": "{{description}}",
        "qty": 1,
        "rate": 100,
        "income_account": "{{default_income_account}}",
        "cost_center": "{{default_cost_center}}"
      }
    ]
  }'
```

---

### 4.2 Get Sales Invoice By ERP ID

```http
GET {{base_url}}/api/resource/Sales Invoice/{{sales_invoice_name}}
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -X GET "{{base_url}}/api/resource/Sales Invoice/{{sales_invoice_name}}" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}"
```

---

### 4.3 Cancel Sales Invoice

```http
POST {{base_url}}/api/method/frappe.client.cancel
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "doctype": "Sales Invoice",
  "name": "{{sales_invoice_name}}"
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/method/frappe.client.cancel" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "doctype": "Sales Invoice",
    "name": "{{sales_invoice_name}}"
  }'
```

---

## 5. Item Requests

### 5.1 Get Item By Item Code

```http
GET {{base_url}}/api/resource/Item/{{item_code}}
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -X GET "{{base_url}}/api/resource/Item/{{item_code}}" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}"
```

---

### 5.2 Get Item Groups

Used when creating missing service items.

```http
GET {{base_url}}/api/resource/Item Group?filters={{filters}}&fields={{fields}}&limit_page_length=5
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**cURL**

```bash
curl -G "{{base_url}}/api/resource/Item Group" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data-urlencode 'filters=[["is_group","=",0]]' \
  --data-urlencode 'fields=["name"]' \
  --data-urlencode "limit_page_length=5"
```

---

### 5.3 Create Item

```http
POST {{base_url}}/api/resource/Item
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "item_code": "{{item_code}}",
  "item_name": "{{item_code}}",
  "item_group": "{{item_group}}",
  "is_stock_item": 0
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/resource/Item" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "item_code": "{{item_code}}",
    "item_name": "{{item_code}}",
    "item_group": "{{item_group}}",
    "is_stock_item": 0
  }'
```

---

## 6. Payment Entry Requests

### Create Payment Entry

```http
POST {{base_url}}/api/resource/Payment Entry
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "doctype": "Payment Entry",
  "payment_type": "Receive",
  "party_type": "Customer",
  "party": "{{customer_erp_id}}",
  "posting_date": "{{payment_date}}",
  "paid_amount": 100,
  "received_amount": 100,
  "target_exchange_rate": 1,
  "paid_to_account_currency": "{{currency}}",
  "company": "{{company}}",
  "paid_to": "{{incoming_payment_account}}",
  "reference_no": "{{transaction_id}}",
  "reference_date": "{{payment_date}}",
  "references": [
    {
      "reference_doctype": "Sales Invoice",
      "reference_name": "{{sales_invoice_name}}",
      "allocated_amount": 100
    }
  ]
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/resource/Payment Entry" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "doctype": "Payment Entry",
    "payment_type": "Receive",
    "party_type": "Customer",
    "party": "{{customer_erp_id}}",
    "posting_date": "{{payment_date}}",
    "paid_amount": 100,
    "received_amount": 100,
    "target_exchange_rate": 1,
    "paid_to_account_currency": "{{currency}}",
    "company": "{{company}}",
    "paid_to": "{{incoming_payment_account}}",
    "reference_no": "{{transaction_id}}",
    "reference_date": "{{payment_date}}",
    "references": [
      {
        "reference_doctype": "Sales Invoice",
        "reference_name": "{{sales_invoice_name}}",
        "allocated_amount": 100
      }
    ]
  }'
```

---

## 7. Journal Entry Requests

### 7.1 Create Journal Entry For General Expense

```http
POST {{base_url}}/api/resource/Journal Entry
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "doctype": "Journal Entry",
  "posting_date": "{{posting_date}}",
  "voucher_type": "Journal Entry",
  "company": "{{company}}",
  "user_remark": "{{remark}}",
  "accounts": [
    {
      "account": "{{debit_account}}",
      "debit_in_account_currency": 100,
      "company": "{{company}}"
    },
    {
      "account": "{{credit_account}}",
      "credit_in_account_currency": 100,
      "company": "{{company}}"
    }
  ]
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/resource/Journal Entry" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "doctype": "Journal Entry",
    "posting_date": "{{posting_date}}",
    "voucher_type": "Journal Entry",
    "company": "{{company}}",
    "user_remark": "{{remark}}",
    "accounts": [
      {
        "account": "{{debit_account}}",
        "debit_in_account_currency": 100,
        "company": "{{company}}"
      },
      {
        "account": "{{credit_account}}",
        "credit_in_account_currency": 100,
        "company": "{{company}}"
      }
    ]
  }'
```

---

### 7.2 Create Journal Entry For Property Expense

This request is used for `plt_exp`.

**Accounts Logic**

- Debit expense account with amount before tax, or total amount if no tax account.
- Debit tax account when tax amount exists.
- Credit treasury account with total amount.

```http
POST {{base_url}}/api/resource/Journal Entry
Content-Type: application/json
Accept: application/json
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

**Body**

```json
{
  "doctype": "Journal Entry",
  "posting_date": "{{posting_date}}",
  "voucher_type": "Journal Entry",
  "company": "{{company}}",
  "user_remark": "{{expense_code}} — {{memo}}",
  "accounts": [
    {
      "account": "{{expense_account}}",
      "debit_in_account_currency": 100
    },
    {
      "account": "{{tax_account}}",
      "debit_in_account_currency": 15
    },
    {
      "account": "{{treasury_account}}",
      "credit_in_account_currency": 115
    }
  ]
}
```

**cURL**

```bash
curl -X POST "{{base_url}}/api/resource/Journal Entry" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: token {{erp_api_key}}:{{erp_api_secret}}" \
  --data '{
    "doctype": "Journal Entry",
    "posting_date": "{{posting_date}}",
    "voucher_type": "Journal Entry",
    "company": "{{company}}",
    "user_remark": "{{expense_code}} — {{memo}}",
    "accounts": [
      {
        "account": "{{expense_account}}",
        "debit_in_account_currency": 100
      },
      {
        "account": "{{tax_account}}",
        "debit_in_account_currency": 15
      },
      {
        "account": "{{treasury_account}}",
        "credit_in_account_currency": 115
      }
    ]
  }'
```

---

## 8. Request Summary Table

| # | Purpose | Method | Endpoint |
|---:|---|---|---|
| 1 | Login | POST | `/api/method/login` |
| 2 | Test logged user | GET | `/api/method/frappe.auth.get_logged_user` |
| 3 | Get Customer | GET | `/api/resource/Customer/{name}` |
| 4 | Search Customer | GET | `/api/resource/Customer?filters=...` |
| 5 | List Customer Groups | GET | `/api/resource/Customer Group?filters=...` |
| 6 | List Territories | GET | `/api/resource/Territory?filters=...` |
| 7 | Create Customer | POST | `/api/resource/Customer` |
| 8 | Create Sales Invoice | POST | `/api/resource/Sales Invoice` |
| 9 | Get Sales Invoice | GET | `/api/resource/Sales Invoice/{name}` |
| 10 | Cancel Sales Invoice | POST | `/api/method/frappe.client.cancel` |
| 11 | Get Item | GET | `/api/resource/Item/{item_code}` |
| 12 | List Item Groups | GET | `/api/resource/Item Group?filters=...` |
| 13 | Create Item | POST | `/api/resource/Item` |
| 14 | Create Payment Entry | POST | `/api/resource/Payment Entry` |
| 15 | Create Journal Entry | POST | `/api/resource/Journal Entry` |

---

## 9. Notes

- All `POST`, `PUT`, and `PATCH` requests use `Content-Type: application/json`.
- Token authentication uses:

```http
Authorization: token {{erp_api_key}}:{{erp_api_secret}}
```

- Password authentication uses session cookies from `/api/method/login`.
- The code disables SSL verification in cURL using:
  - `CURLOPT_SSL_VERIFYPEER => false`
  - `CURLOPT_SSL_VERIFYHOST => false`

For production, it is safer to enable SSL verification if the ERPNext server has a valid SSL certificate.
