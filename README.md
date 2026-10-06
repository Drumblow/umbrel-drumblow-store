# Drumblow — loja comunitária do umbrelOS

Loja comunitária com os manifestos dos apps. O código dos apps não fica aqui: este repositório só
tem o manifesto, o compose, o ícone e os ganchos de instalação. As imagens são publicadas no
GitHub Container Registry e fixadas por digest.

## Como adicionar no umbrelOS

App Store → ⋯ (canto superior direito) → **Community App Stores** → cole
`https://github.com/Drumblow/umbrel-drumblow-store` → **Add**.

## Apps

| ID | Nome | Porta |
|---|---|---|
| `drumblow-painel` | Painel do Escritório | 8552 |
| `drumblow-leiloes` | Leilões — Coletor e Painel | 8790 |

### drumblow-leiloes

Ao contrário dos outros apps, **não usa imagem própria**: roda na imagem oficial `node:22-alpine`
e o código (5 casas + painel + valores) é copiado depois da instalação para
`~/umbrel/app-data/drumblow-leiloes/src` (montado em `/projetos` dentro dos containers). Assim nada
do projeto precisa ser publicado em registry. Enquanto o código não estiver lá, os containers
reiniciarão em laço — o gancho `hooks/pre-install` já deixa as pastas prontas.

Depois de instalar, no PC que tem o código:

```bash
cd Projetos/auction-dashboard && bash deploy/publicar.sh          # código
cd Projetos/auction-dashboard && bash deploy/publicar.sh --dados  # + semeia os 6 bancos
```

Os bancos ficam em `.../src/<projeto>/data/`, ficam fora do backup do umbrelOS (arquivo vivo) e o
serviço `backup` do próprio app grava cópias consistentes em `.../backups/` a cada 6 horas.
