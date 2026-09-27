# PulsePanda Analytics — Google Tag Manager template

Adds [PulsePanda](https://pulsepanda.dev) analytics, session replay and heatmaps to a site through Google Tag Manager, without editing the site's code.

## Install

1. In Tag Manager, open **Templates → Tag Templates → Search Gallery** and add **PulsePanda Analytics**.
   (Before the gallery listing is live: **Templates → New → ⋮ → Import**, and pick `template.tpl` from this repository.)
2. **Tags → New**, choose **PulsePanda Analytics**, and paste your **Site key** (PulsePanda → Settings → Sites; it is the `data-project` value in the install snippet).
3. Trigger: **Initialization - All Pages**. Save, then publish the container.

## dataLayer events

Tick **Send dataLayer events as custom events** to send events your site already pushes to Tag Manager as PulsePanda custom events:

```js
dataLayer.push({ event: 'purchase', ecommerce: { transaction_id: 'T1', value: 49.5, currency: 'USD', items: [/* … */] } });
gtag('event', 'generate_lead', { value: 10 });
```

arrive as `purchase` with `ecommerce.transaction_id`, `ecommerce.value`, `ecommerce.currency`, `ecommerce.items_count`, and `generate_lead` with `value`.

- Leave **Only these events** empty to send every event, or list names (`purchase,generate_lead`).
- Skipped: Tag Manager's own `gtm.*` events and fields, `page_view` (PulsePanda records page views itself), callbacks, `send_to`, `user_data`, and any field whose name looks like contact details (email, phone, address, password).
- Events wait for analytics consent like any other PulsePanda event.

## Consent Mode

To load PulsePanda only after consent, open the tag's **Advanced Settings → Consent Settings**, choose **Require additional consent for tag to fire**, and add `analytics_storage`.

## License

Apache 2.0
