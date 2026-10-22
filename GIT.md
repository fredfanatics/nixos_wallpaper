# Git-Spickzettel (Einzelnutzer, nur `main`)

## Einmalig einrichten

```
git config --global user.name "Dein Name"
git config --global user.email "deine@mail.de"
git config --global init.defaultBranch main
git config --global core.editor vim
git config --global alias.save '!git add -A && git commit -m'
git config --global alias.st 'status -sb'
git config --global alias.sync '!git add -A && git commit -m "$1" && git push && :'
```

Prüfen: `git config --global --list`

## Neues Repo

```
cd projektordner
git init -b main
echo "*.zip" >> .gitignore
git save "Erster Stand"
```

Mit GitHub verbinden (privates Repo vorher auf GitHub anlegen):

```
git remote add origin git@github.com:fredfanatics/REPO.git
git push -u origin main
```

Bestehendes Repo holen: `git clone git@github.com:fredfanatics/REPO.git`

## Täglicher Ablauf

```
git st                    # Was hat sich geändert? Unerwartetes dabei?
git save "Beschreibung"   # Alles festhalten
git push                  # Hochladen (oder gleich: git sync "Beschreibung")
```

Faustregel: Commit, sobald etwas funktioniert, und vor jedem riskanten Schritt
(bei NixOS: vor `nixos-rebuild switch`).

## Vor dem Push prüfen

```
git log --oneline
```

Alles oberhalb der Zeile mit `origin/main` ist noch nicht veröffentlicht
und lässt sich gefahrlos korrigieren.

## Rettungsbefehle

| Situation | Befehl |
|---|---|
| Alle noch nicht gepushten Commits zurücknehmen, Dateien bleiben | `git reset --soft origin/main` |
| Datei aus dem nächsten Commit nehmen, Datei bleibt | `git restore --staged datei` |
| Datei aus dem Repo entfernen, lokal behalten | `git rm --cached datei` |
| Uncommittete Änderungen verwerfen (ENDGÜLTIG!) | `git restore .` |
| Auf alten Stand zurück (Historie bleibt) | `git restore --source=abc1234 .` dann `git save "zurück auf abc1234"` |
| Merge abbrechen | `git merge --abort` |
| Was habe ich zuletzt getan? | `git reflog` |
| Änderungen kurz parken / zurückholen | `git stash` / `git stash pop` |

## Versehentlich einen Branch angelegt

```
git branch                   # Wo bin ich?
git switch main
git merge andererbranch
git branch -d andererbranch
```

## GitHub-Grenzen

- Max. 100 MB pro Datei per Git (25 MB per Web-Upload), Repo am besten unter 1 GB.
- Große Dateien vor dem Commit prüfen:
  `find . -path ./.git -prune -o -type f -size +90M -print`
- Web-Upload gilt erst nach Klick auf "Commit changes".

## SSH-Zugang zu GitHub

```
ssh-keygen -t ed25519 -C "rechnername"
cat ~/.ssh/id_ed25519.pub     # Zeile bei GitHub unter Settings → SSH keys eintragen
ssh -T git@github.com         # Test
```

## Nie committen

Passwörter, Tokens, WLAN-Keys, ZIP-Archive, große Binärdateien.
Bei `/etc/nixos` vor dem ersten Push die Dateien auf Geheimnisse prüfen.
