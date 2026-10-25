# prognosisherba-painel

Painel de planejamento do **PrognosisHerba**, onde o gestor vê a previsão
de crescimento da grama, gera o cronograma de poda e acompanha as
equipes em campo.

| Tela | O que mostra |
|---|---|
| **Previsões IA** | Altura prevista por região, semana a semana, com o limiar de poda marcado |
| **Otimização** | Roda o solver para o número de equipes informado e publica o resultado para as equipes |
| **Cronograma** | Grade equipe × dia útil, com o prazo e a folga de cada local |
| **Agrupamento** | Regiões agrupadas por perfil de crescimento (k-means) |
| **Monitoramento** | Progresso ao vivo do que as equipes já marcaram como concluído |

Faz parte de um conjunto de repositórios:

| Repositório | Papel |
|---|---|
| [prognosisherba-visao](https://github.com/Zinorblatant-cpu/prognosisherba-visao) | Estação de medição por webcam (grade em cm, teorema de Pick) + API de leituras |
| [prognosisherba-api](https://github.com/Zinorblatant-cpu/prognosisherba-api) | Previsões, solver e alocação publicada. **Este painel precisa dela no ar** |
| **prognosisherba-painel** (este) | Painel de planejamento do gestor |
| [prognosisherba-podadores](https://github.com/Zinorblatant-cpu/prognosisherba-podadores) | App das equipes em campo |
| [prognosisherba-docs](https://github.com/Zinorblatant-cpu/prognosisherba-docs) | Documentação completa: arquitetura e modelos matemáticos |

## Rodando

Node.js 20 ou mais novo, com a
[prognosisherba-api](https://github.com/Zinorblatant-cpu/prognosisherba-api)
rodando em `http://127.0.0.1:8002`.

```bash
cp .env.example .env
npm install
npm run dev        # http://localhost:5173
```

| Variável | Padrão | O que faz |
|---|---|---|
| `VITE_API_BASE_URL` | `http://127.0.0.1:8002` | Endereço da API. É embutida no build: se mudar, rode o build de novo |

## Scripts

| Comando | O que faz |
|---|---|
| `npm run dev` | Servidor de desenvolvimento |
| `npm run build` | Checagem de tipos + build de produção em `dist/` |
| `npm test` | Testes (Vitest + Testing Library) |
| `npm run test:coverage` | Testes com cobertura; reprova abaixo de 100% |
| `npm run lint` | Lint com oxlint |

## Stack

React 19 · Vite 8 · TypeScript · Tailwind 4 · Recharts · React Router 7

## Estrutura

```
src/
├── pages/               Home, PrevisoesIA, Otimizacao, Cronograma,
│                        Agrupamento, Monitoramento, Equipe
├── components/layout/   AppShell (gaveta no mobile) + Sidebar
├── components/ui/       Card, Button, Tag, KpiCard, Estado, Calendario
├── state/               AlocacaoContext (alocação gerada, em memória)
└── lib/                 api, calendario, types
```

## Hospedagem

O build gera arquivos estáticos em `dist/`. A hospedagem precisa
redirecionar todas as rotas para `index.html` (`/* → /index.html`),
senão recarregar uma página como `/previsoes` dá 404.

A alocação gerada fica só na memória do navegador: recarregar a página
limpa o Cronograma até rodar a Otimização de novo. O que foi publicado
para as equipes fica guardado na API.
