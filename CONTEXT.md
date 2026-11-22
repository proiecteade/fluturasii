# CONTEXT — 🦋 Fluturașii — portalul părinților

> Citește-l la începutul fiecărui chat despre acest repo; actualizează-l la final.

## Ce e
Portalul părinților pentru grupa mică B „Fluturașii” (Grădinița 246): evenimente, lista de necesare a grupei, plăți (hrană, dans, engleză etc.) și fondul grupei, administrat de Adelina. Include și pagina mos-craciun.html.

- **Adresă:** https://fluturasii.vercel.app
- **Repo:** proiecteade/fluturasii (branch main) — mutat din hellomeri/fluturasii pe 12 noiembrie 2026

## Structură
`index.html` (aplicația, PWA), `mos-craciun.html`, `api/` (funcții Vercel: `push.js` pentru notificări web-push, `keepalive.js`), `img/`, `manifest.webmanifest`, `sw.js`, `vercel.json` (cron zilnic /api/keepalive la 06:00 UTC), iconițe.

## Unde stau datele
Toate datele sunt în Supabase (citite direct din index.html cu supabase-js). Cheile secrete pentru notificări stau în variabilele de mediu din Vercel, nu în repo.

## Zile de naștere
- `copii.data_nasterii` (date). Părinții o completează prin funcția `seteaza_zi_nastere` (pe Acasă sau din numele de sus). La doamne se păstrează doar ziua și luna (anul 1904 = „fără an”).
- Pe Acasă: „La mulți ani!” în ziua respectivă și lista celor din următoarele 14 zile. Tab nou „Zile de naștere” (#zile): calendar pe 12 luni, începând mereu cu luna curentă.

## Notificări
- Același abonament pe telefon (tabelul `push_abonamente`): `email` completat = dispozitiv de administratoare (primește noutățile de la părinți, ca înainte); `copil_id` completat = notificări de părinte. Pe telefonul Adelinei sunt amândouă.
- Părinții: portalul le cere pornirea la prima vizită (și din nou după 7 zile dacă amână); toate tipurile pornite implicit; le opresc sau debifează din numele de sus. Categorii: `alege` (item nou), `plata` (strângere, comandă, lună de opțional), `zile` (doar în ziua respectivă; cele din următoarele 14 zile apar doar pe site).
- Fluxul: triggerul `push_nou` pune noutățile în `push_coada`; `push_proceseaza()` rulează la 2 minute (pg_cron, job „fluturasii-push”), le adună, sare peste ce a fost șters și trimite doar între 8:00 și 21:00 (ora României). Cine a adăugat ceva nu primește notificare pentru el.
- Trimiterea fizică: /api/push pe Vercel (cheile vin din tabelul `secrete`).

## Cum se publică
Vercel publică automat din branch-ul main. Hellomeri.go.ro nu e implicat (pagina /gradinita vine din repo-ul separat proiecteade/gradinita).

## Administrare fără cod (din portal, logată ca administratoare)
- **✏️ Texte** (bara de admin): pune chenar punctat pe titluri, subtitluri, mesaje, file și tot ghidul; apeși și schimbi. Se salvează în tabelul `texte` (cheie → valoare); dacă o cheie lipsește, se folosește textul din cod. „Vezi toate textele” = listă cu căutare. Mini-format: rând gol = paragraf, **îngroșat**, [[buton]].
- **⚙️ Mai mult**: plată nouă cu produse (`comenzi`, cu „ascunsă” / „închisă” / „un singur pachet” / „pentru toți”), opțional nou (cu „banii merg direct la grădiniță” = `optionale.direct`), lună nouă fără Excel (ședințe pe zile sau pe zile de prezență), strângere de bani, anul școlar (`setari.an_scolar`; dacă e gol se folosește cel din cod), anunț/copii/doamne + răspunsul de la intrare (`texte` cheia `intrare.raspuns`, mai multe răspunsuri separate prin virgulă).
- La opționale, administratoarea apasă pe buline (prezent/absent) sau pe „N zile ✏️”.
- Codul nu mai are nimic „bătut” pentru ChatGPT (acum `comenzi.ascuns`) sau hrană (acum `optionale.direct`).

## „Am făcut”
- `inscrieri.facut_la`; funcția `marcheaza_facut(id, copil, on)` — o poate apăsa părintele copilului ales pe telefon sau administratoarea. Apare la înscrieri (Preiau / Mă înscriu) și la lunile „Pe rând, lunar”; pe Acasă trece din „Ce ai tu de făcut” în „Gata”. Administratoarea primește notificare.

## Unde sunt cheile (fără valori)
- Cheile Supabase și cheile pentru notificări (web-push / VAPID): în Vercel → proiectul fluturasii → Settings → Environment Variables; unele sunt și în tabelul `secrete` din Supabase (exclus intenționat din backup).
- Parola bazei de date Supabase: în Supabase → proiectul fluturasii → Settings → Database.
- Tokenul serverului (Hermes, read-only pe proiecteade, expiră 27 oct 2027): pe server, `~/.config/proiecteade/token`.
- Recomandare: toate cheile de mai sus și parolele conturilor (GitHub, Vercel, Supabase) într-un manager de parole.

## Backup
- Baza de date Supabase: săptămânal, în repo-ul privat `proiecteade/backup` (`supabase/fluturasii/`).

## Cum lucrăm
- Proiectul Claude: **🦋 Fluturașii** — chatul 🛠 Features pentru cod/design, chatul 📝 Conținut doar pentru date.
- Claude face tot git-ul (commit, push) și verifică pagina live după publicare. Adelina nu urcă nimic manual.
- La finalul fiecărei sesiuni, Claude adaugă o linie în „Jurnal” mai jos.

## Jurnal
- 2026-11-12: repo mutat în organizația proiecteade; creat CONTEXT.md și HERMES.md.
- 2026-11-12: decis — fără copie pe hellomeri.go.ro/fluturasii (doar Vercel).
- 2026-11-12: adăugat „Unde sunt cheile” și „Backup”.
- 2026-11-14: Poze Atelierul Moșului — adăugat în descriere termenul de alegere a pachetului (25 noiembrie, inclusiv). Doar date în Supabase.
- 2026-11-15: zile de naștere (completare, „La mulți ani!”, calendar anual) + notificări pentru toți părinții.
- 2026-11-15: fără notificare cu 2 săptămâni înainte de zilele de naștere (doar pe site); notificare doar în ziua respectivă.
- 2026-11-15: calendarul zilelor de naștere pornește cu luna curentă (12 luni); scos mesajul de sub câmpul de dată.
- 2026-11-15: adăugat evenimentul „Sărbătorim Moș Nicolae” (4 dec., figurine Decorella, strângerea vine mai târziu); la „Cadouri împachetate și etichetate” — mențiunea că toți copiii primesc același cadou. Doar date în Supabase.
- 2026-11-15: reparat cardurile din „Evenimente următoare”: rândurile noi din descriere se păstrează, linkurile se pot apăsa și nu mai ies din chenar.
- 2026-11-17: adăugată plata obligatorie „Figurine de pictat · Moș Nicolae” (30 lei/copil, până pe 25 nov., cont ING) la De plată. Cod: plățile obligatorii se văd acum (doar ChatGPT rămâne ascuns); eticheta „Pentru toți” la plățile obligatorii.
- 2026-11-22: reconciliere cu extrasul ING 1–22 nov.: confirmate în portal 11 plăți la poze, 16 la figurine, engleza oct. pentru Ilieș. Rămase de lămurit: transferurile de 300 (17 nov.) și 318 lei (19 nov.), cerneala imprimantă 183,96 lei (nu e în buget), cadoul domnișoarei Raluca (598 lei, nu e în portal), 20 lei rămași la caiete EDU.
- 2026-11-22: trecută în buget cheltuiala „Cerneală imprimantă” 183,96 lei (16 nov., cont). Transferurile 300 + 318 lei = cadoul domnișoarei Raluca (nu se ține în portal).
- 2026-11-22: totul editabil din portal (✏️ Texte, ⚙️ Mai mult: plăți cu produse, opționale, luni fără Excel, an școlar, doamne, răspunsul de la intrare); butonul „Am făcut” la înscrieri și la lunile pe rând.
- 2026-11-22: intrat în buget restul de 8 lei de la cadoul domnișoarei Raluca (cu partea lui Vlad, 28 lei, socotită plătită de Adelina).
- 2026-11-22: Vlad (Pîrvu) — plătite figurine și poze (glob); cadoul inclus.
- 2026-11-22: evenimentele pot avea interval de date (`sectiuni.data_pana`, câmpul „Până pe” la editare); rămân la „următoare” până trece ultima zi, cu „În desfășurare, până pe …”.
