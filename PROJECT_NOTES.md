# Zapikanka — პროექტის ჩანაწერი (გაგრძელებისთვის)

ბოლო განახლება: 2026-06-04

## საიტი
- Live: https://zapikanka.vercel.app/
- Repo: **otar1983/zapikanka** (GitHub) → Vercel ავტომატურად დებს (~1 წთ)
- ერთი ფაილი: `index.html` (~15 MB, inline CSS+JS+Firebase, base64 სურათები)

## როგორ გავაგრძელო სხვა კომპზე
1. ჩამოტვირთე `index.html` GitHub-იდან (otar1983/zapikanka) — ეს არის სრული პროექტი ყველა ცვლილებით.
2. რედაქტირება ლოკალურად, შემდეგ ატვირთვა GitHub-ზე (Add file → Upload files) → Vercel ავტომატურად დებს.
3. მონაცემები (მენიუ, იუზერები, კონფიგი) **Firebase-შია (ღრუბელი)** — ნებისმიერი კომპიდან ხელმისაწვდომი.

## Firebase (backend)
- პროექტი: **zapikanka-8f074** (ძველი freshgrill-be926 აღარ გამოიყენება)
- Firestore public rules (`if true`), Auth არ არის — მხოლოდ admin პაროლი
- კოლექციები: `orders`, `users`, `config/site`, `config/menu`, `config/addons`, `config/cashback`
- მენიუ ფაილის `const MENU`-დან იხატება (fbLoadMenu გათიშულია). ახალი პროდუქტი: ადმინში დაამატე → „HTML გადმოწერა" → ატვირთე GitHub-ზე.

## სურათები — Cloudinary (NOT Firebase Storage)
- cloud: `dipsci6vz`, unsigned preset: `zapikanka` (public/უსაფრთხო)
- ლოგო (ჰორიზონტალური): https://res.cloudinary.com/dipsci6vz/image/upload/v1780485460/n9stu6qyazj2tmgq1uft.png

## ადმინ-პანელი
- დამალულია მომხმარებლისგან (navbar-ში ღილაკი არ ჩანს)
- წვდომა: (1) URL `?admin` ან `#admin`, ან (2) ლოგოზე 5-ჯერ სწრაფად დაჭერა
- პაროლი: `zapikanka2024`

## გაკეთებული ცვლილებები (2026-06)
1. ✅ ჰორიზონტალური ლოგო (Cloudinary)
2. ✅ ფილიალის არჩევა checkout-ში (დინამიკური, SITE_CFG-დან)
3. ✅ ლოგინის ბაგი (var USERS = window.USERS)
4. ✅ პროგრეს-ბარები გათხელდა
5. ✅ Checkout-ის ბაგი — ორი cart სისტემა (`cart` + `window.customCart`) გაერთიანდა openCheckout/refreshCheckoutSummary/placeOrder-ში
6. ✅ ადმინი დამალულია მომხმარებლისგან (secret URL + ლოგო 5-tap)
7. ✅ კალათა ჩატვირთვაზე ცარიელია (initApp ასუფთავებს — exportHTML ბაკავდა DOM-ს)
8. ✅ საიტი მთავარ გვერდზე იხსნება (initApp: goTo('home'))
9. ✅ ლოგინის შემდგომი სტატუს-ბარი დაწვრილდა
10. ✅ სესია ინახება reload-ის შემდეგ (localStorage zp_user + restoreSession)
11. ✅ შენახული მისამართები (პროფილში სია + checkout ჩიპები; logged-in → Firebase, guest → localStorage zp_addresses)
12. ✅ Login-ში „დამიმახსოვრე" ჩექბოქსი + პროფილში თვალსაჩინო „🚪 გასვლა"
13. ✅ სასმელი/ფრი/სოუსი/დესერტი → `+`-ზე პირდაპირ კალათაში (openCust-ში NO_CUSTOM სია); ზაპიეკანკა/ვეგი → მორგების ფანჯარა

## მნიშვნელოვანი არქიტექტურა
- **ორი cart:** `cart` (მარტივი {id:qty}) + `window.customCart` (მორგებული ზაპიეკანკა). checkout ორივეს ითვალისწინებს.
- **exportHTML** = `outerHTML` → ჩაბეჭდავს მიმდინარე DOM-ს. ამიტომ კალათა/აქტიური გვერდი ჩატვირთვაზე initApp-ში ისევ უსაფრთხო მდგომარეობას უბრუნდება.

## დარჩენილი (TODO)
- სოც. ბმულები ისევ /freshgrill-ზე — ნამდვილი Zapikanka handle-ები სჭირდება
- SMS ვერიფიკაცია — ფასიანია (Firebase Blaze ან ქართული SMS-გეიტვი + serverless function)
