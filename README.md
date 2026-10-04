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

## Google Consent Mode v2

**If PulsePanda's cookie banner is your consent tool** (PulsePanda → Cookies), the banner can drive Google Consent Mode v2 for your Google tags (GA4, Google Ads, Floodlight):

1. In the PulsePanda tag, tick **Set Google Consent Mode defaults (PulsePanda banner)**.
2. Change the tag's trigger to **Consent Initialization - All Pages**, so the defaults are set before any Google tag runs.
3. Optional: list the regions where consent is required (**Deny by default only in these regions**, for example `AT,BE,DE,FR,GB`). Visitors elsewhere default to granted. Leave it empty to default to denied everywhere.

The tag sets every signal to `denied`, then restores a returning visitor's saved choice. Whenever a visitor decides in the banner, the PulsePanda script sends `gtag('consent', 'update', …)` and pushes a `pulsepanda_consent` event, which you can use as a trigger for tags that wait for consent.

| PulsePanda category | Google signals |
|---|---|
| Analytics | `analytics_storage` |
| Marketing | `ad_storage`, `ad_user_data`, `ad_personalization` |
| Preferences | `functionality_storage`, `personalization_storage` |
| Strictly necessary | `security_storage` (always granted) |

A category the banner doesn't offer stays denied: turn on **Marketing** in the banner if you run Google Ads tags.

Don't add consent requirements to the PulsePanda tag itself in this setup. The script shows the banner and holds its own tracking until the visitor agrees.

**If another consent tool sets Google consent**, leave the checkbox off. To load PulsePanda only after consent, open the tag's **Advanced Settings → Consent Settings**, choose **Require additional consent for tag to fire**, and add `analytics_storage`.

## License

Apache 2.0
