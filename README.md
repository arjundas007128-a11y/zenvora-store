# ZENVORA Shopping Website

A beginner-friendly static storefront for GitHub Pages with Supabase Auth + Postgres backend.

## Files
- `index.html`, `styles.css`, `app.js`: storefront, product search/categories, cart, customer auth, checkout, order list.
- `admin.html`, `admin.js`: authenticated admin console, product creation, order status/payment-status updates.
- `config.js`: public Supabase project URL + anon key placeholders.
- `supabase/schema.sql`: database tables, RLS policies, safe order-creation function, sample products.

## 1. Create Supabase backend
1. Create a Supabase project at https://supabase.com/ (free tier availability/limits can change).
2. Open **SQL Editor** → New query.
3. Paste all of `supabase/schema.sql` and run it.
4. In **Project Settings → API**, copy the Project URL and `anon` / publishable key.
5. Edit `config.js` and replace `YOUR_SUPABASE_PROJECT_URL` and `YOUR_SUPABASE_ANON_KEY`.
6. Never use the `service_role` key in `config.js` or any browser code.

## 2. Create the admin account safely
1. Open the website after deployment and use Create account with the admin email you control.
2. Confirm the email if Supabase asks.
3. In Supabase → SQL Editor, run this with your own exact email:
   `update public.profiles set role = 'admin' where email = 'YOUR_ADMIN_EMAIL';`
4. Sign in at `/admin.html`. Admin access is checked against the database role.
5. Do not share the admin password. If you are under 18, have a parent/guardian manage business, payment, and account ownership.

## 3. Publish on GitHub Pages
1. Create a new GitHub repository named `zenvora-store`.
2. Upload the files and folders from this ZIP (keep `schema.sql` inside `supabase/`).
3. Open repository **Settings → Pages**.
4. Under Build and deployment choose **Deploy from a branch**.
5. Select `main` and `/ (root)`, then Save.
6. Wait for GitHub Pages to publish. Open the URL shown on the Pages settings screen.

## 4. Configure Supabase Auth
1. Supabase → Authentication → URL Configuration.
2. Set Site URL to your GitHub Pages URL.
3. Add that same URL to Redirect URLs if email confirmation is enabled.
4. Test sign-up, sign-in, sign-out, and a customer order.

## 5. Add products and manage orders
- Sign in as the admin at `https://YOUR-GITHUB-USERNAME.github.io/zenvora-store/admin.html`.
- Add products using the Add product form.
- New customer orders appear in the Admin panel.
- Change order status as the order moves through pending → confirmed → processing → shipped → delivered.

## Payments: important limits
- Cash on Delivery is supported as an order method.
- UPI in this starter is **manual / pending verification only**. It does not verify a payment or accept UPI PINs.
- For real online payment confirmation, integrate a reputable payment gateway (for example, Razorpay) through a secure server-side function. Never put gateway secret keys in GitHub Pages JavaScript. Payment merchant onboarding and bank account ownership should be handled by an adult/guardian where required.
- Do not collect card numbers, UPI PINs, banking passwords, or OTPs in this website.

## Security notes
- Supabase Row Level Security is enabled on all tables.
- Customer order reads are restricted to their own authenticated account.
- Admin operations require a profile role of `admin`.
- The order RPC recalculates totals from database product prices; it ignores client-supplied prices.
- The browser anon key is public by design; RLS is the security boundary.
- Review and test policies before accepting real orders. This is a starter project, not a production audit.
- GitHub Pages is static hosting; secrets must never be placed in frontend files.

## Demo mode
Before Supabase is configured, the home page displays sample products. Sign-in and checkout are intentionally disabled until the backend is connected.
