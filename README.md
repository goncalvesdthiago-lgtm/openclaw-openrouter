# OpenRouter no OpenClaw: passo a passo

Como ligar o [OpenClaw](https://docs.openclaw.ai) ao [OpenRouter](https://openrouter.ai) para usar várias IAs com uma única chave e um único saldo.

- **OpenRouter** é um balcão único de modelos: você cria uma chave, coloca crédito e essa chave dá acesso a centenas de modelos de várias empresas. Não tem mensalidade por modelo; o crédito é descontado conforme o uso.
- **OpenClaw** é um assistente de código aberto que roda no seu computador. É com ele que você conversa e é ele que executa as tarefas, usando por trás o modelo que você escolher.

Juntando os dois, trocar de IA vira uma linha de configuração.

> Comandos conferidos na ajuda do OpenClaw 2026.7.1-2, em outubro de 2026. Em versões mais novas, confirme com `openclaw <comando> --help`.

## O que você precisa

- Uma conta no OpenRouter com crédito (20 dólares já dão para testar bastante coisa em texto).
- Node instalado no computador.
- Um pouco de terminal.

## 1. Crie a chave no OpenRouter

1. Entre em [openrouter.ai](https://openrouter.ai) e faça login.
2. Abra a página de chaves: [openrouter.ai/keys](https://openrouter.ai/keys).
3. Crie uma chave e dê um nome que você reconheça, como "OpenClaw".
4. Copie a chave na hora. Ela só aparece uma vez e começa com `sk-or-`.

Trate a chave como senha: não cole em chat, print, repositório nem documento compartilhado.

## 2. Coloque crédito

Na área de créditos do OpenRouter, adicione o valor que quiser.

Vale definir um limite de gasto na própria chave. Assim, se algo sair do controle, ela não consome o saldo inteiro.

## 3. Instale o OpenClaw

Se ainda não tiver instalado:

```bash
npm install -g openclaw@latest
openclaw --version
```

## 4. Ligue o OpenClaw ao OpenRouter

Na primeira vez, rode a configuração inicial já escolhendo o OpenRouter. Ele vai pedir a chave que você copiou:

```bash
openclaw onboard --auth-choice openrouter-api-key
```

Se o OpenClaw já estava instalado com outro provedor, use a configuração interativa e escolha OpenRouter nas credenciais:

```bash
openclaw configure
```

O OpenClaw também aceita a chave direto no comando (`--openrouter-api-key`), mas assim ela fica gravada no histórico do terminal. Prefira o caminho em que ele pergunta.

## 5. Escolha o modelo padrão

O nome do modelo segue o formato `openrouter/empresa/modelo`, igual ao que aparece na página do modelo no OpenRouter.

Para o dia a dia, um modelo rápido e barato resolve a maior parte dos pedidos:

```bash
openclaw models set openrouter/google/gemini-2.5-flash
```

Se preferir que o OpenRouter escolha o modelo a cada pedido:

```bash
openclaw models set openrouter/auto
```

## 6. Deixe um modelo reserva

O reserva assume quando o principal falha ou está fora do ar:

```bash
openclaw models fallbacks add openrouter/google/gemini-2.5-flash-lite
```

## 7. Confira e teste

```bash
openclaw models status
openclaw chat
```

O primeiro comando mostra o modelo ativo e os reservas. O segundo abre a conversa no terminal: mande uma pergunta simples para ver se responde.

Se algo falhar, rode o diagnóstico:

```bash
openclaw doctor
```

## 8. Acompanhe o gasto

A página de atividade do OpenRouter ([openrouter.ai/activity](https://openrouter.ai/activity)) mostra quanto cada modelo consumiu. É ali que você descobre qual modelo vale o preço para cada tipo de tarefa.

## Qual modelo para quê

A graça do OpenRouter é ter um especialista para cada tarefa na mesma chave. Alguns exemplos do catálogo:

| Tarefa | Modelos |
|---|---|
| Texto, código e raciocínio | Claude, GPT, Gemini |
| Tarefa simples e barata | Gemini Flash, modelos abertos como Qwen e Gemma |
| Imagem | Nano Banana (o modelo de imagem do Gemini) |
| Vídeo | Kling, Veo, Wan |
| Apresentador falando um texto | Avatar da HeyGen |
| Voz e narração | GPT Audio |
| Trilha sonora | Lyria |

O catálogo muda com frequência. Confira os nomes e os preços atuais em [openrouter.ai/models](https://openrouter.ai/models).

## Cuidados

- **O crédito acaba mais rápido nos modelos caros.** Vídeo é o mais caro de todos. Deixe o modelo barato como padrão e chame o caro só quando a tarefa pedir.
- **O OpenClaw executa ações no seu computador.** Leia o aviso de risco da configuração inicial antes de aceitar.
- **A chave é uma senha.** Se ela vazar, apague no painel do OpenRouter e crie outra.

## Fontes

- [Connect OpenClaw to OpenRouter](https://openrouter.ai/blog/tutorials/openclaw-openrouter/), tutorial do OpenRouter
- [OpenRouter na documentação do OpenClaw](https://docs.openclaw.ai/openrouter)

---

Escrito por Thiago Dias.
