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
- 2026-11-24: Moș Crăciun ales — Cum Vă Place, ora 17:00, 500 lei; itemul „Moș Crăciun de închiriat” marcat rezolvat.
- 2026-11-24: la „Moș Crăciun de închiriat” trecută familia Pavel ca responsabilă; plată nouă la De plată „Moș Crăciun · Serbarea de Crăciun” — 23 lei/copil, pentru toți (506 lei; restul de 6 lei intră în buget când plătesc toți).
- 2026-11-24: stare nouă la itemi: „⏸ În pauză” (nu mai apare în „de făcut”/termene; card gri). Baza de date: permisă starea `pauza` în tabelul stari.
- 2026-11-24: plată ascunsă „Cadou de Crăciun · jocuri Orchard” (`cadou-craciun`): 27 de jocuri Orchard 2–3 ani de la Gameology (fără extensiile Shopping List), cu preț −15%; fiecare părinte alege un joc. Cod: produsele pot avea poză (`img`) → alegerea apare ca grilă cu poze și link „Vezi jocul”; câmp „Poză” în formularul de produse; texte „joc” în loc de „pachet” când sunt poze.
- 2026-11-24: modul „Cadouri” la plăți (`comenzi.cadouri`): fără „Nu vreau niciun joc”, fiecare joc ales are 🛒 comandă comună / 🏠 comand eu acasă, frați/surori (max 6) cu joc din listă sau „aducem noi cadoul” (`comenzi_copii.detalii`, salvate prin funcția `comanda_detalii`); totaluri „Alese până acum” și „Comanda comună” (doar ce intră la grup). Produsele pot avea `grup` (titluri în grilă). Cadou de Crăciun: 48 de jocuri (27 Orchard 2–3 ani + 6 Gameology propuse, −15%; 15 Smart Games de pe De-a joaca), încă ascunsă.
- 2026-11-25: cadou de Crăciun — șterse alegerile de test ale lui Vlad; jocurile sunt în 2 secțiuni: Gameology (toate, crescător după prețul cu −15%) și De-a joaca (Smart Games, crescător).
- 2026-11-25: cadou Crăciun — termen alegere și plată 30 nov. (comenzile pe 1 dec.), plata în contul ING, codul MOMS4MOMS la Gameology; încă ascunsă.
- 2026-11-25: cadou Crăciun — pusă live pentru părinți (nu mai e ascunsă).
- 2026-11-25: „De plată” reorganizat: bandă cu totul de plată al copilului, secțiunea „🔁 Lunare” (cardurile opționalelor, detaliul dedesubt) și „🧾 Alte plăți” (comenzile, cu „Încheiate” strânse). Strângerile de bani nu mai apar la De plată, doar în Bugetul grupei.
- 2026-11-25: cadouri — statistici noi: câți copii din 22 au ales + frați/surori; comanda comună pe magazine (Gameology / De-a joaca) cu număr, sumă și jocuri ×bucăți.
- 2026-11-25: strângerile de bani s-au mutat la De plată → „Alte plăți” (deschise în listă, închise la „Încheiate”); butonul „+ Strângere” e acolo. Bugetul grupei arată doar soldul, totalurile și mișcările. Linkurile vechi #buget/<strângere> duc la De plată.
- 2026-11-25: De plată se deschide strâns (doar carduri și rânduri); detaliul apare la tap și se închide la al doilea tap. Linkurile directe (#deplata/<id>) și banda de sus deschid direct plata respectivă.
- 2026-11-25: pe Acasă, lista „✓ Gata” e strânsă; se deschide la tap.
