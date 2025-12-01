# Side Quest THM - Guide Complet

## Point de départ

Message initial trouvé dans `/home/mcskidy/Documents/read-me-please.txt` :

```bash
root@tbfc-web01:/home/mcskidy/Documents$ cat read-me-please.txt 
```

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
Read the ledger's older pages.

3)
When pixels sleep, their tails sometimes whisper plain words.
Listen to the tail.

Find the fragments, join them in order, and use the resulting passcode
to decrypt the message I left. Be careful — I had to be quick,
and I left only enough to get help.

~ McSkidy
```

## Étape 1 : Connexion au compte eddi_knapp

```bash
su - eddi_knapp
```

Mot de passe : `S0mething1Sc0ming`

## Étape 2 : Recherche du Fragment 1

**Indice 1 :** "I ride with your session, not with your chest of files. Open the little bag your shell carries when you arrive."

Cela fait référence aux variables d'environnement.

```bash
eddi_knapp@tbfc-web01:~$ env | grep -Ei 'egg|fragment|part|clue|token|pass|key|secret'
PASSFRAG1=3ast3r
```

**Fragment trouvé :** `PASSFRAG1=3ast3r`

## Étape 3 : Recherche du Fragment 2

**Indice 2 :** "The tree shows today; the rings remember yesterday. Read the ledger's older pages."

Cela fait référence à Git (tree = arbre Git, rings = historique).

### Exploration du dépôt Git caché

```bash
eddi_knapp@tbfc-web01:/$ cd ~/.secret_git
eddi_knapp@tbfc-web01:~/.secret_git$ ls -la
total 12
drwx------  3 eddi_knapp eddi_knapp 4096 Nov 11 12:07 .
drwxr-x--- 18 eddi_knapp eddi_knapp 4096 Dec  1 17:55 ..
drwx------  8 eddi_knapp eddi_knapp 4096 Nov 11 12:07 .git
```

### Vérification de l'historique Git

```bash
eddi_knapp@tbfc-web01:~/.secret_git$ git log
commit e924698378132991ee08f050251242a092c548fd (HEAD -> master)
Author: mcskiddy <mcskiddy@robco.local>
Date:   Thu Oct 9 17:20:11 2025 +0000

    remove sensitive note

commit d12875c8b62e089320880b9b7e41d6765818af3d
Author: McSkidy <mcskiddy@tbfc.local>
Date:   Thu Oct 9 17:19:53 2025 +0000

    add private note
```

```bash
eddi_knapp@tbfc-web01:~/.secret_git$ git log --all --oneline
e924698 (HEAD -> master) remove sensitive note
d12875c add private note
```

### Affichage du commit contenant la note

```bash
eddi_knapp@tbfc-web01:~/.secret_git$ git show d12875c
commit d12875c8b62e089320880b9b7e41d6765818af3d
Author: McSkidy <mcskiddy@tbfc.local>
Date:   Thu Oct 9 17:19:53 2025 +0000

    add private note

diff --git a/secret_note.txt b/secret_note.txt
new file mode 100755
index 0000000..060736e
--- /dev/null
+++ b/secret_note.txt
@@ -0,0 +1,5 @@
+========================================
+Private note from McSkidy
+========================================
+We hid things to buy time.
+PASSFRAG2: -1s-
```

**Fragment trouvé :** `PASSFRAG2=-1s-`

## Étape 4 : Recherche du Fragment 3

**Indice 3 :** "When pixels sleep, their tails sometimes whisper plain words. Listen to the tail."

Cela fait référence aux métadonnées d'images (pixels = images, tails = fin de fichier).

### Vérification du répertoire Pictures

```bash
eddi_knapp@tbfc-web01:~/Pictures$ cat .easter_egg
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@%%%%@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@#+==+*@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@%+=+*++@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@*++**+#@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@%%#*====+#%@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@#*===-===#@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@%*++:-+====*%@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@%*===++++===-+*#######%%@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@%*+===+++==::-=========+*#%@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@%%#**+======-:-==--==-==+*%@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@%*+======---=+===------=#%@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@%**+=-=====-==+==-====--=*%@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@%***+++==--=====+=----=-=#@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@%#**++=--=====++====----*@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@%*+=-:=++**++**+=-::--*@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@#+=:.+#***=*#=--::-=-=%@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@%%*+-:+%#+++=++=:::==--*%@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@%*+=--*@#++===::::::::=#%@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@%%%##*#%%%####***#*#####%%@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@%%###%%%%%%%%%%##%%##%%@@@@@@@@@@@@

~~ HAPPY EASTER ~~~
PASSFRAG3: c0M1nG
```

**Fragment trouvé :** `PASSFRAG3=c0M1nG`

## Étape 5 : Déchiffrement du vault GPG

### Récapitulatif des fragments

- PASSFRAG1 = `3ast3r`
- PASSFRAG2 = `-1s-`
- PASSFRAG3 = `c0M1nG`

**Mot de passe complet :** `3ast3r-1s-c0M1nG`

### Déchiffrement du fichier

```bash
echo "3ast3r-1s-c0M1nG" | gpg --batch --passphrase-fd 0 -d /home/eddi_knapp/Documents/mcskidy_note.txt.gpg
```

**Résultat :**

```
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase
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
and I can smell chocolate in the air. Something big is coming.  — McSkidy

---

When the wishlist is corrected, the site will show a block of ciphertext. This ciphertext can be decrypted with the following unlock key:

UNLOCK_KEY: 91J6X7R4FQ9TQPM9JX2Q9X2Z

To decode the ciphertext, use OpenSSL. For instance, if you copied the ciphertext into a file /tmp/website_output.txt you could decode using the following command:

cat > /tmp/website_output.txt
openssl enc -d -aes-256-cbc -pbkdf2 -iter 200000 -salt -base64 -in /tmp/website_output.txt -out /tmp/decoded_message.txt -pass pass:'91J6X7R4FQ9TQPM9JX2Q9X2Z'
cat /tmp/decoded_message.txt

Sorry to be so convoluted, I couldn't risk making this easy while King Malhare watches. — McSkidy
```

## Étape 6 : Correction de la wishlist

### Passage en root

```bash
sudo su
```

### Modification du fichier wishlist

```bash
cat > /home/socmas/2025/wishlist.txt << 'EOF'
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
EOF
```

### Vérification

```bash
wc -l /home/socmas/2025/wishlist.txt
sed -n '1,200p' /home/socmas/2025/wishlist.txt
```

**Résultat :**

```
10 /home/socmas/2025/wishlist.txt
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
```

## Étape 7 : Accès au site web

Accéder au site via Firefox : `http://ipvm:8080`

Après le rechargement de la page, le site affiche la wishlist corrigée et le texte chiffré :

![Page web avec wishlist corrigée](image)

**Texte chiffré récupéré :**

```
U2FsdGVkX1/7xkS74RBSFMhpR9Pv0PZrzOVsIzd38sUGzGsDJOB9FbybAWod5HMsa+WIr5HDprvK6aFNYuOGoZ60qI7axX5Qnn1E6D+BPknRgktrZTbMqfJ7wnwCExyU8ek1RxohYBehaDyUWxSNAkARJtjVJEAOA1kEOUOah11iaPGKxrKRV0kVQKpEVnuZMbf0gv1ih421QvmGucErFhnuX+xv63drOTkYy15s9BVCUfKmjMLniusI0tqs236zv4LGbgrcOfgir+P+gWHc2TVW4CYszVXlAZUg07JlLLx1jkF85TIMjQ3B91MQS+btaH2WGWFyakmqYltz6jB5DOSCA6AMQYsqLlx53ORLxy3FfJhZTl9iwlrgEZjJZjDoXBBMdlMCOjKUZfTbt3pnlHWEaGJD7NoTgywFsIw5cz7hkmAMxAIkNn/5hGd/S7mwVp9h6GmBUYDsgHWpRxvnjh0s5kVD8TYjLzVnvaNFS4FXrQCiVIcp1ETqicXRjE4T0MYdnFD8h7og3ZlAFixM3nYpUYgKnqi2o2zJg7fEZ8c=
```

## Étape 8 : Déchiffrement du message final

### Suppression du fichier temporaire (si existant)

```bash
rm /tmp/website_output.txt
```

### Création du fichier avec le ciphertext

```bash
cat > /tmp/website_output.txt
U2FsdGVkX1/7xkS74RBSFMhpR9Pv0PZrzOVsIzd38sUGzGsDJOB9FbybAWod5HMsa+WIr5HDprvK6aFNYuOGoZ60qI7axX5Qnn1E6D+BPknRgktrZTbMqfJ7wnwCExyU8ek1RxohYBehaDyUWxSNAkARJtjVJEAOA1kEOUOah11iaPGKxrKRV0kVQKpEVnuZMbf0gv1ih421QvmGucErFhnuX+xv63drOTkYy15s9BVCUfKmjMLniusI0tqs236zv4LGbgrcOfgir+P+gWHc2TVW4CYszVXlAZUg07JlLLx1jkF85TIMjQ3B91MQS+btaH2WGWFyakmqYltz6jB5DOSCA6AMQYsqLlx53ORLxy3FfJhZTl9iwlrgEZjJZjDoXBBMdlMCOjKUZfTbt3pnlHWEaGJD7NoTgywFsIw5cz7hkmAMxAIkNn/5hGd/S7mwVp9h6GmBUYDsgHWpRxvnjh0s5kVD8TYjLzVnvaNFS4FXrQCiVIcp1ETqicXRjE4T0MYdnFD8h7og3ZlAFixM3nYpUYgKnqi2o2zJg7fEZ8c=
```

Appuyer sur **CTRL+D** pour terminer la saisie.

### Déchiffrement avec OpenSSL

```bash
openssl enc -d -aes-256-cbc -pbkdf2 -iter 200000 -salt -base64 \
  -in /tmp/website_output.txt \
  -out /tmp/decoded_message.txt \
  -pass pass:'91J6X7R4FQ9TQPM9JX2Q9X2Z'
```

### Lecture du message déchiffré

```bash
cat /tmp/decoded_message.txt
```

**Résultat final :**

```
Well done — the glitch is fixed. Amazing job going the extra mile and saving the site. Take this flag THM{w3lcome_2_A0c_2025}

NEXT STEP:
If you fancy something a little...spicier....use the FLAG you just obtained as the passphrase to unlock:
/home/eddi_knapp/.secret/dir

That hidden directory has been archived and encrypted with the FLAG.
Inside it you'll find the sidequest key.
```

## Flag obtenu

**THM{w3lcome_2_A0c_2025}**

## Prochaine étape suggérée

Utiliser le flag comme passphrase pour déchiffrer `/home/eddi_knapp/.secret/dir`
