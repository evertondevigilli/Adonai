# Adonai Card Game — reinício do projeto (Alpha)

## Arquivos deste pacote

- `gamefile.json`: primeira configuração do jogo para TCG Arena, adaptada do arquivo `Game_FaithBattle.json` e das regras ADONAI CARD GAME v1.2.
- `cards.json`: arquivo estrutural vazio por enquanto. Nenhuma carta foi inventada nem importada sem confirmação dos dados e das URLs de imagem oficiais.
- `LEIA-ME-PRIMEIRO.md`: este roteiro e as limitações conhecidas.

## Zonas adaptadas do Faith Battle

| Nome no Faith Battle | Chave usada no Adonai | Nome exibido |
|---|---|---|
| `Sabedorias` | `Caracteristicas` | Características |
| `Mana` | `ZonaDeCaracteristicas` | Zona de Características |
| `Preparacao` | `essencias` | Essências |
| `Extra` | `Identidade` | Identidade |
| `CampoBatalha` | `CampoBatalha` | Campo de Batalha |

A documentação atual do TCG Arena exige que as chaves de zona e as referências no layout sejam nomes sem espaços. Por isso, a chave técnica `ZonaDeCaracteristicas` aparece na mesa como **Zona de Características**.

Os fundos e versos gráficos próprios do Faith Battle foram removidos para não usar assets de outro jogo. Quando identificarmos os assets oficiais do Adonai, eles poderão ser adicionados.

## Mulligan configurado

- Mão inicial: 6 cartas.
- Mulligan parcial: escolhe-se de 0 a 6 cartas.
- Até 3 mulligans.
- Sequência configurada: `toBottom` → `shuffle` → `draw`.

A sequência foi organizada para corresponder às regras v1.2: devolver ao baralho, embaralhar e comprar a mesma quantidade. O projeto Faith Battle fornecido usa a sequência `toBottom` → `draw` → `shuffle`; não copiei essa ordem literalmente porque ela não corresponde à ordem descrita no arquivo de regras atualizado.

## Baralho e regras que ainda precisam de tratamento próprio

As regras v1.2 exigem 40 cartas singleton no conjunto inicial e permitem reduzir o baralho para 30–40 cartas antes da partida. O `deckRuleset` deste protótipo exige 40 cartas únicas na construção inicial. O procedimento para escolher 30–40 e encaminhar as restantes à Reserva ainda não foi automatizado.

Também ainda não estão automatizados integralmente:
- escolher e revelar exatamente 1 Campeão, 1 Território e 1 Templo;
- a sequência especial pré-jogo multijogador de olhar e reorganizar cartas do topo;
- Consagração, Profanação e movimentação dos eixos morais;
- pagamento e geração de Essência segundo as regras de `Preparado`/`Despreparado`;
- custos de Pecado por colocação de marcadores de dano no Território;
- pilha, prioridade, alvos, combate simultâneo, dano e ações baseadas no estado;
- movimento automático correto de Milagres e Pecados Momentâneos, Duradouros e Vínculos. No protótipo, Personagens e Artefatos resolvidos vão ao Campo de Batalha; Milagres e Pecados vão ao Cemitério por padrão. Os Duradouros e Vínculos podem precisar ser movidos/vinculados manualmente.

`drawOnStart: false` implementa o caso de o primeiro jogador não comprar na primeira etapa de Compra. A regra do Adonai diferencia partidas de dois jogadores e partidas multijogador; o ajuste multijogador ainda requer automação e teste específicos.

## Catálogo oficial e imagens

Fonte indicada pelo usuário: https://adonaicardgame.com.br/cards/?sort=number-asc

A visualização web consultada expôs a interface e os filtros do catálogo, mas não forneceu uma lista estruturada completa das cartas nem as URLs diretas das imagens. Por isso, `cards.json` está vazio e `gamefile.json` usa URLs provisórias. Isso evita inventar nomes, custos, atributos ou endereços de imagem.

Para usar o jogo, ainda será necessário:
1. obter, a partir do catálogo oficial, o identificador, nome, tipo, custo, subtipo, atributos e URL de imagem de cada carta;
2. converter cada registro para o formato do TCG Arena;
3. obter a URL pública da imagem do verso de carta (se houver) e substituir o valor provisório em `cards.cardBack`;
4. hospedar `gamefile.json`, `cards.json` e imagens em endereços públicos e substituir `cards.dataUrl`.

Formato mínimo exigido pelo TCG Arena para cada entrada de `cards.json`:

```json
{
  "id-da-carta": {
    "id": "id-da-carta",
    "face": {
      "front": {
        "name": { "name": "Nome oficial da carta" },
        "type": "Personagem",
        "cost": 1,
        "image": "https://URL-PUBLICA-DA-IMAGEM"
      }
    },
    "name": "Nome oficial da carta",
    "type": "Personagem",
    "cost": 1
  }
}
```

O exemplo é apenas o formato técnico e não representa uma carta real do Adonai.

## URLs provisórias

Em `gamefile.json`:
- `cards.dataUrl` ainda está com `https://SEU-USUARIO.github.io/SEU-REPOSITORIO/cards.json`;
- `cards.cardBack` ainda está com uma URL de exemplo.

Essas URLs precisam ser substituídas antes de importar o jogo no TCG Arena. Não é necessário conservar um nome de repositório anterior; use o endereço que realmente for criado no GitHub Pages.

## Fontes de estrutura técnica

- Documentação geral do TCG Arena: https://documentation.tcg-arena.fr/
- Mulligan, turnos e configuração inicial: https://documentation.tcg-arena.fr/gameplay
- Zonas, layout, nomes sem espaços e zonas reservadas: https://documentation.tcg-arena.fr/sections
- Formato de `cards.json`: https://documentation.tcg-arena.fr/content-files
