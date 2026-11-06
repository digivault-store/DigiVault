# DigiVault

A React + TypeScript + Vite + Tailwind starter for a premium digital marketplace.

## Included
- Black / navy / gold responsive storefront
- Search, category filters, product cards and cart
- Checkout creates `pending_payment` orders only
- Admin login through Supabase Auth
- Admin product, order and payment-settings screens
- Supabase schema migration and starter RLS policies
- No payment gateway integration; all payment providers remain inactive

## 1. Run locally
Install Node.js 20+ and then:

```bash
npm install
cp .env.example .env
npm run dev
```

Open the local URL printed by Vite.

## 2. Connect Supabase
1. Create a Supabase project.
2. In Project Settings > API Keys, copy the Project URL and publishable key.
3. Put them in `.env`:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
4. In Supabase SQL Editor, run `supabase/migrations/001_initial_schema.sql`.
5. In Authentication > Users, create your own admin user.
6. Copy that user's UUID, then run this SQL in SQL Editor:
   ```sql
   update public.profiles set role='admin' where id='YOUR_AUTH_USER_UUID';
   ```
7. Restart the Vite server after changing `.env`.

Never put a Supabase `service_role` or secret key in browser code or `.env` variables prefixed with `VITE_`.

## 3. Important production notes
This is a starter implementation, not yet a production-ready commerce backend.
- The current checkout needs a signed-in customer account to insert an order under the supplied RLS policies. Add customer signup/login before accepting real customer orders.
- The frontend demo fallback is local in-memory state and does not persist across refresh.
- Complete order creation should be moved into a trusted server-side Edge Function or backend transaction that validates product prices and writes `orders` + `order_items` atomically. Do not trust totals sent by a browser.
- The admin UI is protected by Supabase Auth, but the RLS policies are the actual data security boundary. Review and test them before launch.
- Do not mark orders paid manually from the browser. When you later integrate an approved payment provider, verify signed webhooks server-side before changing status to `paid`.
- Only list digital goods/services you are authorized to sell and that comply with provider terms and applicable law.
- Add privacy policy, terms, refund policy, customer support details, rate limiting, backups, logging and monitoring before public launch.

## 4. Deploy
Build with `npm run build`. Deploy the generated `dist/` directory to a static host such as Vercel or Netlify. Add the same two environment variables in the hosting provider's project settings. Add your deployed domain to Supabase Authentication URL Configuration / Redirect URLs when auth redirects are enabled.

## Domain
You can first deploy on a free hosting subdomain, test everything, and connect a custom domain later.
DigiVault deployment setup
