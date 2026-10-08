# kea-init-config (fuente para recipe Launchpad → PPA)

Paquete nativo `3.0 (native)`, versión `1.0.0`, para
`ppa:jhonpork445566/kea-init-config` (series `noble`).

Layout (raíz = raíz del paquete, lo que exige la recipe):

```text
kea-init-config/
├── src/kea-config-init      # el binario (bash, 755)
└── debian/
    ├── changelog            # kea-config-init (1.0.0) noble
    ├── control              # debhelper-compat 13, ${misc:Depends}
    ├── copyright
    ├── rules                # dh, 755
    ├── kea-config-init.install   # src/kea-config-init usr/sbin
    ├── kea-config-init.postinst  # con #DEBHELPER#
    └── source/format        # 3.0 (native)
```

## Subir a Launchpad por git (una vez)

```bash
cd ~/kea-init-config
git init -b main
git add -A && git commit -m "kea-config-init 1.0.0 para PPA noble"
# 1. Crea el repo vacío en: https://code.launchpad.net/~jhonpork445566/+git/kea-init-config  (New repository)
# 2. Sube tu clave SSH en: https://launchpad.net/~/+editsshkeys  (cat ~/.ssh/id_ed25519.pub)
git remote add launchpad git+ssh://jhonpork445566@git.launchpad.net/~jhonpork445566/+git/kea-init-config
git push -u launchpad main
```

## Recipe (build automático al PPA, sin dput)

1. Abre `https://code.launchpad.net/~jhonpork445566/+git/kea-init-config/+new-recipe`
   (o desde el PPA → Create recipe).
2. Nombre: `kea-init-config-noble`, PPA destino:
   `~jhonpork445566/ubuntu/kea-init-config`, serie `noble`.
3. Pega esta receta (`recipe.txt` de este repo):

```text
# git-build-recipe format 0.4 deb-version 1.0.0+git{revno}
lp:~jhonpork445566/+git/kea-init-config
```

4. `Request build(s)` → `noble amd64` → en 10-30 min aparece en
   `.../+archive/ubuntu/kea-init-config/+packages` como `Published`.
5. Cada `git push` posterior → `Request build` de nuevo (o marca
   Autobuild diario). Para nueva versión: `dch -i` (ej. `1.0.1`,
   nativo SIN guion), commit, push, build.

## Instalación mundial (cuando la recipe publique)

```bash
sudo add-apt-repository -y ppa:jhonpork445566/kea-init-config
sudo apt update && sudo apt install -y kea-config-init
sudo kea-config-init
```
