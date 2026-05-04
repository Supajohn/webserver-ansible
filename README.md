# webserver-ansible

Ansible-rooli joka asentaa ja konfiguroi valitun webpalvelimen (Apache2 tai Nginx) automaattisesti — valintasi mukaan interaktiivisesti asennuksen aikana. Tukee myös HTTPS-sertifikaatin hankkimista Certbotin avulla.

**Tekijät:** Joonas Laine | Juho Talvitie | Joni Voong  
**Lisenssi:** GNU General Public License v3.0

---

### Lopputulos

![Kuvakaappaus asennetusta sivusta](https://private-user-images.githubusercontent.com/194824773/586863889-24ea2698-dafe-4291-843d-7707da36c1e4.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3Nzc4Nzk5NzYsIm5iZiI6MTc3Nzg3OTY3NiwicGF0aCI6Ii8xOTQ4MjQ3NzMvNTg2ODYzODg5LTI0ZWEyNjk4LWRhZmUtNDI5MS04NDNkLTc3MDdkYTM2YzFlNC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwNTA0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDUwNFQwNzI3NTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yMTAwMzQ4NTVhNmFjZjUxNWE5ZTQzNjBhOThmNjBhZTA2ZjhmMzhjNTFlMTVkMzZkZTk2ZThkYmUzNWMyMzQxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.Ke5mvHDj2QVeWznrnvgLyC2sUxnrp50xxao8zndYlgA)

---

## Vaatimukset

- Ansible 2.9+
- Kohdekone: Debian/Ubuntu
- Certbot-asennusta varten: oikea domain joka osoittaa kohdekoneen IP:hen

## Asennus

```bash
git clone https://github.com/Havikki/webserver-ansible.git
cd webserver-ansible
```

## Käyttöönotto

1. Muokkaa `inventory`-tiedostoon kohdekoneesi osoite
2. Aja playbook:

```bash
ansible-playbook -i inventory site.yml -K
```

Playbook kysyy asennuksen alussa:
- **Webpalvelin:** `apache2` tai `nginx`
- **Certbot:** haluatko HTTPS-sertifikaatin (`yes`/`no`)
- **Domain:** domain-nimi (tarvitaan vain certbotille)
- **Sähköposti:** certbot-ilmoituksia varten (tarvitaan vain certbotille)

Asennus on idempotentti — voit ajaa playbookin uudelleen ilman sivuvaikutuksia.

## Rakenne

```
webserver-ansible/
├── site.yml                        # pääplaybook (vars_prompt)
├── inventory                       # kohdekone(et)
└── roles/
    └── webserver/
        ├── tasks/
        │   ├── main.yml            # package-file-service -runko
        │   ├── apache.yml          # apache2-spesifi asennus
        │   ├── nginx.yml           # nginx-spesifi asennus
        │   └── certbot.yml         # HTTPS-sertifikaatti
        ├── handlers/
        │   └── main.yml            # reload webserver
        └── templates/
            ├── index.html.j2       # dynaaminen etusivu
            ├── apache.conf.j2      # VirtualHost-konfiguraatio
            └── nginx.conf.j2       # server block -konfiguraatio
```

## Lisenssi

GNU General Public License v3.0 — katso [LICENSE](LICENSE)
