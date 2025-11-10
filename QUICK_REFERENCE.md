# Quick Reference Guide - Shopify Order Confirmation Email

## 📋 Table of Contents
1. [File Overview](#file-overview)
2. [Key Sections](#key-sections)
3. [Important Variables](#important-variables)
4. [Common Modifications](#common-modifications)
5. [Troubleshooting](#troubleshooting)

---

## File Overview

**File**: `shopify-order-confirmation-complete.liquid`  
**Type**: Shopify Email Notification Template  
**Purpose**: Order confirmation emails sent to customers  
**Lines**: ~950  
**Size**: ~65KB

---

## Key Sections

### 1. Configuration & Variables (Lines 1-50)
```liquid
{% assign delivery_method_types = ... %}
{% assign has_split_cart = ... %}
{% capture email_title %}...{% endcapture %}
{% capture email_body %}...{% endcapture %}
```
**Purpose**: Initialize all variables and capture reusable content

### 2. Header (Lines 60-120)
```liquid
<table class="header row">
  - Shop logo/name
  - Order number
  - PO number
</table>
```
**Purpose**: Display branding and order identification

### 3. Main Content (Lines 130-220)
```liquid
<table class="row content">
  - Email title
  - Email body
  - Gift card messages
  - Payment instructions
  - Action buttons
</table>
```
**Purpose**: Primary message and call-to-action

### 4. Order Summary (Lines 230-650)
```liquid
<table class="row section">
  - Line items
  - Pricing breakdown
  - Discounts
  - Totals
</table>
```
**Purpose**: Detailed order information

### 5. Customer Information (Lines 660-800)
```liquid
<table class="row section">
  - Addresses
  - Payment methods
  - Shipping method
</table>
```
**Purpose**: Customer and delivery details

### 6. Footer (Lines 810-850)
```liquid
<table class="row footer">
  - Contact information
  - Legal notices
</table>
```
**Purpose**: Support information and legal compliance

---

## Important Variables

### Shop Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `{{ shop.name }}` | Store name | "My Store" |
| `{{ shop.email }}` | Store email | "support@mystore.com" |
| `{{ shop.url }}` | Store URL | "https://mystore.com" |
| `{{ shop.email_logo_url }}` | Logo URL | "https://cdn.shopify.com/..." |
| `{{ shop.email_accent_color }}` | Brand color | "#FF6B6B" |

### Order Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `{{ order_name }}` | Order number | "#1001" |
| `{{ order_status_url }}` | Order status page | "https://mystore.com/orders/..." |
| `{{ po_number }}` | Purchase order # | "PO-12345" |
| `{{ subtotal_price }}` | Subtotal | 99.99 |
| `{{ total_price }}` | Total | 109.99 |
| `{{ tax_price }}` | Tax amount | 10.00 |
| `{{ shipping_price }}` | Shipping cost | 5.00 |

### Customer Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `{{ customer.first_name }}` | First name | "John" |
| `{{ customer.email }}` | Email | "john@example.com" |
| `{{ shipping_address }}` | Shipping address | Address object |
| `{{ billing_address }}` | Billing address | Address object |

### Line Item Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `{{ line.title }}` | Product name | "T-Shirt" |
| `{{ line.variant.title }}` | Variant | "Large / Blue" |
| `{{ line.quantity }}` | Quantity | 2 |
| `{{ line.final_line_price }}` | Line total | 39.98 |
| `{{ line.image }}` | Product image | Image object |

### Payment Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `{{ transactions }}` | Payment transactions | Array |
| `{{ financial_status }}` | Payment status | "paid" |
| `{{ payment_terms }}` | Payment terms (B2B) | Object |

### Delivery Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `{{ delivery_method }}` | Delivery type | "ship", "pick-up", "local" |
| `{{ shipping_method.title }}` | Shipping method | "Standard Shipping" |
| `{{ delivery_instructions }}` | Special instructions | "Leave at door" |

---

## Common Modifications

### 1. Change Email Title
**Location**: Lines 15-20

```liquid
{% capture email_title %}
  Your custom title here
{% endcapture %}
```

### 2. Customize Email Body Message
**Location**: Lines 25-45

```liquid
{% capture email_body %}
  {% if has_pending_payment %}
    Your custom pending payment message
  {% else %}
    Your custom confirmation message
  {% endif %}
{% endcapture %}
```

### 3. Add Custom Content After Order Summary
**Location**: After line 650

```liquid
<table class="row section">
  <tr>
    <td class="section__cell">
      <center>
        <table class="container">
          <tr>
            <td>
              <h3>Your Custom Section</h3>
              <p>Your custom content here</p>
            </td>
          </tr>
        </table>
      </center>
    </td>
  </tr>
</table>
```

### 4. Modify Footer Text
**Location**: Lines 820-830

```liquid
<p class="disclaimer__subtext">
  Your custom footer text here
  <a href="mailto:{{ shop.email }}">{{ shop.email }}</a>
</p>
```

### 5. Change Button Text
**Location**: Lines 180-190

```liquid
<a href="{{ order_status_url }}" class="button__text">
  Your Custom Button Text
</a>
```

### 6. Add Social Media Links
**Location**: Footer section (after line 830)

```liquid
<table class="row">
  <tr>
    <td style="text-align: center; padding: 20px;">
      <a href="https://facebook.com/yourpage">Facebook</a> |
      <a href="https://instagram.com/yourpage">Instagram</a> |
      <a href="https://twitter.com/yourpage">Twitter</a>
    </td>
  </tr>
</table>
```

### 7. Add Promotional Banner
**Location**: After header (line 120)

```liquid
<table class="row">
  <tr>
    <td style="background-color: #f0f0f0; padding: 15px; text-align: center;">
      <p style="margin: 0;">
        <strong>Free shipping on orders over $50!</strong>
      </p>
    </td>
  </tr>
</table>
```

---

## Troubleshooting

### Issue: Logo Not Displaying
**Solution**: Check that `shop.email_logo_url` is set in Shopify admin
```liquid
{% if shop.email_logo_url %}
  <img src="{{ shop.email_logo_url }}" alt="{{ shop.name }}" width="{{ shop.email_logo_width }}">
{% else %}
  <h1>{{ shop.name }}</h1>
{% endif %}
```

### Issue: Prices Not Showing
**Solution**: Verify currency settings and use correct filters
```liquid
{{ total_price | money }}              {# $99.99 #}
{{ total_price | money_with_currency }} {# $99.99 USD #}
```

### Issue: Line Items Not Rendering
**Solution**: Check for proper loop structure
```liquid
{% for line in line_items %}
  {{ line.title }} - {{ line.quantity }}
{% endfor %}
```

### Issue: Discounts Not Calculating
**Solution**: Ensure discount_applications loop is correct
```liquid
{% for discount_application in discount_applications %}
  {% if discount_application.target_selection == 'all' %}
    {{ discount_application.title }}
  {% endif %}
{% endfor %}
```

### Issue: Email Not Responsive on Mobile
**Solution**: Ensure viewport meta tag is present
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### Issue: Buttons Not Clickable
**Solution**: Check that URLs are properly formatted
```liquid
<a href="{{ order_status_url }}" class="button__text">View Order</a>
```

### Issue: Images Broken in Outlook
**Solution**: Use absolute URLs and specify dimensions
```liquid
<img src="{{ 'notifications/logo.png' | shopify_asset_url }}" 
     width="200" 
     height="50" 
     alt="Logo">
```

---

## Testing Checklist

### Before Deployment
- [ ] Test with sample order
- [ ] Check all variables render
- [ ] Verify calculations are correct
- [ ] Test in multiple email clients
- [ ] Check mobile rendering
- [ ] Verify all links work
- [ ] Test with different order types:
  - [ ] Single item order
  - [ ] Multiple items order
  - [ ] Order with discounts
  - [ ] Order with gift cards
  - [ ] Subscription order
  - [ ] B2B order with payment terms
  - [ ] Split cart order
  - [ ] Order with refunds

### Email Client Testing
- [ ] Gmail (Desktop)
- [ ] Gmail (Mobile)
- [ ] Outlook 2016+
- [ ] Outlook.com
- [ ] Apple Mail (macOS)
- [ ] Apple Mail (iOS)
- [ ] Yahoo Mail
- [ ] Android Email

---

## Quick Tips

### 💡 Tip 1: Use Comments Liberally
```liquid
{% comment %} This section handles gift card logic {% endcomment %}
```

### 💡 Tip 2: Test Variables Before Using
```liquid
{% if variable %}
  {{ variable }}
{% else %}
  Default value
{% endif %}
```

### 💡 Tip 3: Keep Inline Styles for Email Compatibility
```html
<td style="padding: 20px; background-color: #f0f0f0;">
```

### 💡 Tip 4: Use Shopify Filters
```liquid
{{ price | money }}
{{ date | date: "%B %d, %Y" }}
{{ address | format_address }}
```

### 💡 Tip 5: Always Backup Before Editing
Save a copy of the original template before making changes.

---

## Useful Liquid Filters

| Filter | Purpose | Example |
|--------|---------|---------|
| `money` | Format currency | `{{ 99.99 | money }}` → "$99.99" |
| `money_with_currency` | Currency with code | `{{ 99.99 | money_with_currency }}` → "$99.99 USD" |
| `date` | Format date | `{{ order.created_at | date: "%B %d, %Y" }}` |
| `format_address` | Format address | `{{ shipping_address | format_address }}` |
| `img_url` | Generate image URL | `{{ line.image | img_url: 'medium' }}` |
| `upcase` | Uppercase | `{{ "text" | upcase }}` → "TEXT" |
| `downcase` | Lowercase | `{{ "TEXT" | downcase }}` → "text" |
| `capitalize` | Capitalize | `{{ "text" | capitalize }}` → "Text" |
| `replace` | Replace text | `{{ "hello" | replace: 'h', 'j' }}` → "jello" |
| `split` | Split string | `{{ "a,b,c" | split: ',' }}` → ["a","b","c"] |
| `join` | Join array | `{{ array | join: ', ' }}` |
| `size` | Get size | `{{ array | size }}` |
| `plus` | Add | `{{ 5 | plus: 3 }}` → 8 |
| `minus` | Subtract | `{{ 5 | minus: 3 }}` → 2 |
| `times` | Multiply | `{{ 5 | times: 3 }}` → 15 |
| `divided_by` | Divide | `{{ 10 | divided_by: 2 }}` → 5 |

---

## Resources

### Shopify Documentation
- [Liquid Reference](https://shopify.dev/docs/api/liquid)
- [Email Notifications](https://help.shopify.com/en/manual/orders/notifications)
- [Notification Variables](https://shopify.dev/docs/api/liquid/objects)

### Email Testing
- [Litmus](https://litmus.com/) - Email testing platform
- [Email on Acid](https://www.emailonacid.com/) - Email testing
- [Can I Email](https://www.caniemail.com/) - Email client support

### Design Resources
- [Really Good Emails](https://reallygoodemails.com/) - Email inspiration
- [Email Design Reference](https://templates.mailchimp.com/) - Mailchimp templates

---

## Support

For issues with this template:
1. Check Shopify's notification documentation
2. Test in Shopify's email preview
3. Verify all Liquid syntax is correct
4. Check email client compatibility

**Last Updated**: November 10, 2025  
**Version**: 2.0 (Refactored)
