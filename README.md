# Projeto: Simulação Educacional de Malware (Ransomware + Keylogger)

> **Aviso legal e ético**
>
> Este repositório é estritamente educacional. **Nunca** execute scripts que manipulem ou capturem dados em máquinas de produção, sistemas de terceiros, ou sem consentimento explícito. Use **ambientes isolados** (VMs/containers com snapshot) e arquivos **de teste/dummy**. O autor não se responsabiliza por uso indevido.

---

## Índice

1. Visão Geral
2. Objetivos do Projeto
3. Conteúdo do Repositório
4. Ambiente Seguro e Pré-requisitos
5. Estrutura e Descrição dos Scripts (simulados)
6. Como Executar (modo seguro)
7. Cenários de Teste (sugestões)
8. Análise, Detecção e Mitigação
9. Resultados e Documentação
10. Boas práticas de GitHub e Entrega
11. Referências e Recursos úteis
12. Licença

---

## 1. Visão Geral

Este repositório reúne um projeto didático que simula — em ambiente controlado — o comportamento básico de **ransomware** e **keylogger**, com foco em aprendizado sobre:

* Como essas ameaças atuam (conceitualmente);
* Quais artefatos geram (arquivos, logs, mensagens);
* Como detectar, mitigar e se proteger;
* Como documentar experimentos para portfólio (GitHub).

O objetivo **NÃO** é fornecer ferramentas operacionais, e sim criar uma base teórica + experimentos com scripts **não-destrutivos** e com modos `--dry-run` ou `--confirm` para análise.

---

## 2. Objetivos do Projeto

* Entender mecanismos básicos de criptografia aplicada a arquivos (apenas para estudo).
* Simular captura de eventos de teclado em arquivos de teste (APENAS em ambiente controlado).
* Implementar mecanismos de controle para criptografar/descriptografar **somente** arquivos de teste.
* Documentar processos de detecção (logs, assinaturas, comportamento), respostas e medidas de defesa.
* Publicar um repositório com README detalhado, exemplos e reflexões sobre segurança.

---

## 4. Ambiente Seguro e Pré-requisitos

Recomenda-se executar tudo em uma **máquina virtual** ou container isolado (VirtualBox, VMware, QEMU, Docker) com snapshot:

* Python 3.8+
* `virtualenv` (recomendado)
* Bibliotecas opcionais: `cryptography` (Fernet), `pynput` (apenas para testes locais), `smtplib` (biblioteca padrão do Python para envio de e-mail)

Exemplo de instalação (na VM):

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**Notas de segurança**:

* Use contas de e-mail de teste descartáveis para qualquer envio.

---

## 5. Estrutura e Descrição dos Scripts (simulados)

> **Importante:** Os scripts devem ser escritos com segurança: `--dry-run` padrão, confirmação explícita (`--confirm`) para operações destrutivas, e limitação a pastas de teste.

### `ransomware_sim.py` (simulado)

* Objetivo: demonstrar o fluxo de criptografia/descriptografia aplicado apenas a arquivos em `tests/files`.
* Modos e flags sugeridos:

  * `--list-targets` — lista arquivos alvo (sem alterações).
  * `--encrypt --target <dir>` — encripta arquivos dummy (gera `.enc`).
  * `--decrypt --key <keyfile>` — descriptografa arquivos previamente encriptados.
  * `--dry-run` — exibe ações sem alterar arquivos.
  * `--confirm` — permite escrita (obrigatório para operações que alteram arquivos).
* Implementação segura:

  * Gerar chave localmente e armazenar em `tests/keys/` (apenas para demonstração).
  * Produzir arquivo `README_RESCUE.txt` com mensagem de demonstração (sem instruções criminosas reais).

### `keylogger_sim.py` (simulado)

* Objetivo: demonstrar a captura de eventos de teclado em ambiente controlado e registrar em `tests/keylogs/`.
* Modos e flags sugeridos:

  * `--record --duration <seconds>` — grava por N segundos em `tests/keylogs/log.txt`.
  * `--report` — gera estatísticas (número de eventos, tipo de input).
  * `--send --smtp-config <file>` — envia log para e-mail de teste (apenas em laboratório).
* Implementação segura:

  * Arquivo de saída com permissões restritas.
  * Redigir/anonimizar dados sensíveis antes de qualquer compartilhamento.

---

## 6. Como Executar (exemplos seguros)

1. Clone o repositório na VM isolada.
2. Ative o ambiente virtual e instale dependências.
3. Crie arquivos dummy para teste:

```bash
mkdir -p tests/files
echo "arquivo de teste 1" > tests/files/test1.txt
echo "senha: 1234" > tests/files/secret_dummy.txt
```

4. Executar ransomware em modo `--dry-run`:

```bash
python3 scripts/ransomware_sim.py --target tests/files --dry-run
```

5. Após revisar o que será afetado, rodar com confirmação:

```bash
python3 scripts/ransomware_sim.py --target tests/files --encrypt --confirm
```

6. Teste do keylogger (10 segundos):

```bash
python3 scripts/keylogger_sim.py --record --duration 10
```

7. Envio por e-mail (apenas com conta de teste e config apropriada):

```bash
python3 scripts/keylogger_sim.py --send --smtp-config tests/smtp_config_template.json
```

> Sempre verifique se os arquivos afetados estão dentro de `tests/` e que existe snapshot da VM.

---

## 7. Cenários de Teste (sugestões)

* Arquivos: txt, pequenos .docx dummy, imagens.
* Métricas: tempo de execução, uso de CPU/RAM.
* Artefatos: arquivos `.enc`, arquivos de log, `README_RESCUE.txt` (simulado).
* Ferramentas: executar antivírus na VM, coletar logs do sistema, usar `strace`/`procmon` para observar comportamento.

---

## 8. Análise, Detecção e Mitigação

Documente em `docs/` itens como:

* **IoCs (Indicadores de Comprometimento):** padrões de nome de arquivo, extensões geradas, horários de criação, hashes.
* **Como detectar:** monitoramento de criação/alteração maciça de arquivos, processos anômalos, tráfego SMTP não autorizado.
* **Mitigação:** isolar máquina, restaurar backups, varredura com EDR/AV, aplicar patches, rotinas de least-privilege.
* **Resposta:** playbook de incidente (coletar evidências, isolar, analisar, restaurar, comunicar).
* **Prevenção:** backups offline, segmentação de rede, MFA, conscientização do usuário, atualização contínua.

---

## 9. Resultados e Documentação

Inclua em `docs/analysis.md` e nas imagens:

* Screenshots (da VM) mostrando execução e artefatos (sanitizados).
* Logs de execução (sanitizados).
* Checklist de sucesso: por exemplo, arquivos `.enc` criados e descriptografados com sucesso quando a chave correta é usada.
* Reflexão crítica: limitações do modelo simulado, riscos, e como o experimento ilustra vetores reais.

---

## 10. Boas práticas de GitHub e Entrega

* `README.md` com aviso ético no topo.
* `.gitignore` para não subir chaves ou credenciais.
* Scripts com `--dry-run` padrão e confirmação para operações destrutivas.
* Commits pequenos e descritivos (ex.: `feat: adicionar modo dry-run ao ransomware_sim`).
* Adicionar `LICENSE` (ex.: MIT) e observar políticas da sua instituição/curso sobre publicação de conteúdo sensível.

**Texto sugerido para entrega (colar no formulário):**

> Este repositório contém a implementação e documentação **educacional** de dois simuladores: Ransomware (modo controlado e não destrutivo) e Keylogger (registro em arquivo de teste). Todas as execuções devem ser feitas em VM isolada; o repositório inclui instruções, testes, análises de detecção e recomendações de mitigação. Link do repositório: `<COLE_AQUI>`.

---

## 11. Referências e Recursos úteis

* Python — documentação oficial
* `cryptography` (Fernet) — documentação
* `pynput` — documentação de captura de teclado
* `smtplib` — biblioteca padrão Python para envio de e-mails
* Slides do curso: "Simulando um Malware de Captura de Dados Simples em Python"
* GitHub Quick Start

(Adicionar links na versão final do README.)

---

## 12. Licença

Escolha e especifique a licença desejada (ex.: MIT). Reforce que o conteúdo é educacional e que o uso indevido é proibido.

---

> Se desejar, posso também gerar os scripts `ransomware_sim.py` e `keylogger_sim.py` em modo **totalmente seguro** (com `--dry-run` como padrão, operações limitadas a `tests/` e sem envio real), ou preparar o `requirements.txt` e um `smtp_config_template.json`. Quer que eu gere esses arquivos agora?
