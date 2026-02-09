# FALAI

FALAI é um app de transcrição de voz para texto com foco em velocidade, simplicidade e uso diário. A transcrição usa Groq (Whisper Large v3 Turbo) e a correção automática usa Llama (Groq), com estilos de escrita.

## Principais recursos

- Transcrição em tempo real via Groq Whisper Large v3 Turbo
- Correção automática com Llama (Groq) nos estilos: Casual, Pontual, Formal
- Push‑to‑talk com atalho global (padrão: `ctrl+shift`)
- Cola automática no campo ativo (ou apenas copia)
- Barra flutuante de status
- Histórico com busca e filtros
- Estatísticas de uso
- Tema claro/escuro
- Ícone na bandeja do sistema (tray)

## Requisitos

- Python 3.10+
- Microfone configurado
- Chave de API da Groq

## Instalação

```bash
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

## Configuração da API

Crie o arquivo `.env` a partir do exemplo:

```bash
copy .env.example .env
```

Edite o `.env`:

```
GROQ_API_KEY=seu_token_aqui
DEFAULT_LANGUAGE=pt
```

Observação: se você também definir `OPENAI_API_KEY`, o app pode usar fallback automático quando a Groq estiver indisponível.

## Como executar

```bash
python main.py
```

No Windows, você pode usar:

```bash
run_falai.bat
```

## Atalho e modos

- Padrão: `ctrl+shift` (push‑to‑talk)
- Modos: `push_to_talk`, `toggle`, `always_on`

## Configurações (data/config.json)

Exemplo de chaves principais:

```json
{
  "language": "pt",
  "auto_paste": true,
  "activation_mode": "push_to_talk",
  "hotkey": "ctrl+shift",
  "theme": "dark",
  "gpt_correction": true,
  "correction_style": "formal",
  "show_floating": true,
  "dictation_fx": false,
  "mute_music": true
}
```

Notas:
- `correction_style`: `casual`, `neutro` (Pontual), `formal`
- `auto_paste`: se `false`, o texto vai para o clipboard

## Estrutura do projeto

```
FALAI/
├── main.py
├── requirements.txt
├── .env
├── .env.example
├── src/
│   ├── audio/        # gravação e transcrição
│   ├── ui/           # interface PySide6
│   ├── utils/        # hotkeys, histórico, clipboard, etc.
│   └── config/       # settings e validações
└── data/
    └── config.json
```

## Problemas comuns

**Sem transcrição**
- Verifique a `GROQ_API_KEY` no `.env`
- Confira se o microfone está ativo no Windows

**Atalho não funciona**
- Confirme a combinação em `data/config.json`
- Evite conflitos com atalhos do sistema

**Sem áudio detectado**
- Feche apps que possam estar usando o microfone

## Licença

MIT

## Banco de dados (Supabase + Baserow)

Arquivos criados para a nova arquitetura híbrida:

- `database/supabase/schema.sql`
- `database/supabase/rls_policies.sql`
- `database/supabase/seed_plans.sql`
- `docs/baserow_mapping.md`
- `docs/integration_checklist.md`
- `scripts/migration_local_to_cloud.py`
- `src/backend/*` (camada de integração)

Fluxo recomendado:

1. Aplicar SQL no Supabase (`schema`, `RLS`, `seed`).
2. Configurar variáveis em `.env`.
3. Validar login/entitlement via módulos `src/backend`.
4. Rodar migração local em `dry-run` antes do `push`.
