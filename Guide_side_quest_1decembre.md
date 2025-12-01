# GUIDE COMPLET – Side Quest : Fragments et déchiffrement

## 1. Lecture du message de McSkidy

Accéder au répertoire `mcskidy/Documents` et lister les fichiers :

```bash
web01:/home$ cd mcskidy/
root@tbfc-web01:/home/mcskidy$ cd Documents/
root@tbfc-web01:/home/mcskidy/Documents$ ls
read-me-please.txt
root@tbfc-web01:/home/mcskidy/Documents$ ls -la
total 12
drwxr-xr-x  2 mcskidy mcskidy 4096 Oct 29 20:48 .
drwxr-x--- 21 mcskidy mcskidy 4096 Nov 13 17:10 ..
-rw-rw-r--  1 mcskidy mcskidy 1078 Oct 29 20:48 read-me-please.txt
```

Lire le contenu du fichier :

```bash
root@tbfc-web01:/home/mcskidy/Documents$ cat read-me-please.txt
```

Contenu du message :

```
From: mcskidy
To: whoever finds this

I had a short second when no one was watching. I used it.

I've managed to plant a few clues around the account.
If you can get into the user below and look carefully,
those three little "easter eggs" will combine into a passcode
that unlocks a further message that I encrypted in the
/home/eddi_knapp/Documents/ directory.
I didn't want the wrong eyes to see it.

Access the user account:
username: eddi_knapp
password: S0mething1Sc0ming

There are three hidden easter eggs.
They combine to form the passcode to open my encrypted vault.

Clues (one for each egg):

1)
I ride with your session, not with your chest of files.
Open the little bag your shell carries when you arrive.

2)
The tree shows today; the rings remember yesterday.
Read the ledger’s older pages.

3)
When pixels sleep, their tails sometimes whisper plain words.
Listen to the tail.

Find the fragments, join them in order, and use the resulting passcode
to decrypt the message I left. Be careful — I had to be quick,
and I left only enough to get help.

~ McSkidy
```

Ce message explique que le challenge consiste à :

1. Se connecter sur l’utilisateur `eddi_knapp`
2. Trouver trois fragments
3. Combiner les fragments pour obtenir le mot de passe
4. Déchiffrer le fichier GPG `/home/eddi_knapp/Documents/mcskidy_note.txt.gpg`

---

## 2. Connexion à l’utilisateur eddi_knapp

```bash
su - eddi_knapp
```

---

## 3. Fragment 1 – Variables d’environnement

Lister toutes les variables :

```bash
env | less
```

Chercher les variables suspectes :

```bash
env | grep -Ei 'egg|fragment|part|clue|token|pass|key|secret' || true
```

Inspecter l’environnement du shell courant :

```bash
tr '\0' '\n' < /proc/$$/environ | less
```

Filtrer pour détecter du texte lisible :

```bash
tr '\0' '\n' < /proc/$$/environ | egrep -i '^[A-Za-z0-9\-_]{3,}$|egg|pass|part|clue'
```

**Fragment trouvé :**

```
PASSFRAG1=3ast3r
```

---

## 4. Fragment 2 – Historique Git

Se rendre dans le dépôt caché :

```bash
cd ~/.secret_git
ls -la
```

Lister les commits :

```bash
git log
git log --all --oneline
```

Afficher le commit contenant le fragment :

```bash
git show d12875c
```

**Fragment trouvé :**

```
PASSFRAG2: -1s-
```

---

## 5. Fragment 3 – ASCII Art

Afficher le fichier caché :

```bash
cat .easter_egg
```

**Fragment trouvé :**

```
PASSFRAG3: c0M1nG
```

---

## 6. Mot de passe complet

Assembler les fragments dans l’ordre :

```
3ast3r-1s-c0M1nG
```

---

## 7. Déchiffrement du fichier GPG

Tentative simple :

```bash
gpg -d /home/eddi_knapp/Documents/mcskidy_note.txt.gpg
```

Si problème de session :

```bash
echo "3ast3r-1s-c0M1nG" | gpg --batch --passphrase-fd 0 -d /home/eddi_knapp/Documents/mcskidy_note.txt.gpg
```

**Contenu déchiffré :**

```
Congrats — you found all fragments and reached this file.

Below is the list that should be live on the site. If you replace the contents of
/home/socmas/2025/wishlist.txt with this exact list (one item per line, no numbering),
the site will recognise it and the takeover glitching will stop. Do it — it will save the site.

Hardware security keys (YubiKey or similar)
Commercial password manager subscriptions (team seats)
Endpoint detection & response (EDR) licenses
Secure remote access appliances (jump boxes)
Cloud workload scanning credits (container/image scanning)
Threat intelligence feed subscription

Secure code review / SAST tool access
Dedicated secure test lab VM pool
Incident response runbook templates and playbooks
Electronic safe drive with encrypted backups

A final note — I don't know exactly where they have me, but there are *lots* of eggs
and I can smell chocolate in the air. Something big is coming. — McSkidy
```

---

## 8. Déchiffrement du message final du site

Après avoir corrigé le fichier `wishlist.txt`, le site affiche un bloc chiffré. Clé fournie :

```
UNLOCK_KEY: 91J6X7R4FQ9TQPM9JX2Q9X2Z
```

Copier le ciphertext dans un fichier :

```bash
cat > /tmp/website_output.txt
```

Déchiffrement OpenSSL :

```bash
openssl enc -d -aes-256-cbc -pbkdf2 -iter 200000 -salt -base64 \
    -in /tmp/website_output.txt -out /tmp/decoded_message.txt \
    -pass pass:'91J6X7R4FQ9TQPM9JX2Q9X2Z'
```

Afficher le message final :

```bash
cat /tmp/decoded_message.txt
```

---
