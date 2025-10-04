# Projeto Educacional — Simulações seguras: Ransomware & Keylogger (foco em defesa)

> **Aviso importante:** Este repositório tem fins **exclusivamente educacionais e defensivos**. Nenhum script aqui contido deve ser executado fora de um ambiente de laboratório isolado e com autorização. Siga as instruções de segurança abaixo antes de qualquer execução.

## Visão geral
Este projeto documenta a análise e a simulação **segura** de comportamentos associados a ransomware e keyloggers, com foco em:
- Entendimento conceitual das técnicas utilizadas por essas ameaças;
- Construção de contramedidas e regras de detecção (Sysmon, Sigma, YARA);
- Experimentos controlados que **não** realizam ações maliciosas em sistemas alheios;
- Playbook de resposta a incidentes e recomendações de defesa.

> Observação: os scripts de `simulations/` são **inofensivos** (ex.: geração de ficheiros de teste e geração de nota de resgate fictícia). Eles **não cifram ficheiros nem capturam teclas de forma furtiva**.

---

## Estrutura do repositório
- `README.md` — este arquivo.
- `lab-setup/` — instruções para configurar VMs, snapshots e ambiente seguro.
- `simulations/`
  - `generate_test_files.py` — gera ficheiros de teste em `test_files/` para simular criação massiva de ficheiros.
  - `generate_ransom_note.py` — gera um ficheiro `NOTA_RESGATE.txt` com mensagem educativa.
  - *(outros scripts seguros e documentados)*
- `detection/`
  - `sysmon-config.xml` — exemplo de configuração Sysmon.
  - `sigma/` — regras Sigma (mass file creation, ransom note creation, suspicious child spawn).
  - `yara/` — regras YARA educativas.
- `playbooks/`
  - `incident_response.md` — playbook de resposta a incidentes.
- `docs/` — relatório técnico, pseudocódigo e reflexões.
- `slides/` — esboço de apresentação.
- `images/` — prints/screenshots das execuções e dashboards (ex.: alertas no SIEM, execução do script inofensivo, gráficos). **Veja abaixo como estão organizadas as imagens.**

---

## O que acontece — resumo das simulações (em linguagem simples)
- **Geração de ficheiros de teste** (`generate_test_files.py`):
  - Cria uma pasta local chamada `test_files/` e popula com N ficheiros texto.
  - Finalidade: simular um padrão de *criação massiva de ficheiros* para testar detecções comportamentais (ex.: regras Sigma).
  - Segurança: todos os ficheiros são gerados apenas na pasta `test_files/` criada pelo script.

- **Geração de nota de resgate** (`generate_ransom_note.py`):
  - Cria um ficheiro de texto `NOTA_RESGATE.txt` com uma mensagem fictícia.
  - Finalidade: permitir testar detecções por conteúdo (YARA / Sigma) sem causar dano.
  - Segurança: não altera, apaga ou cifra quaisquer outros ficheiros.

- **Regras de detecção e configuração** (`detection/`):
  - Contém exemplos de configuração Sysmon, Sigma rules e YARA rules para identificar padrões como criação massiva de ficheiros, nomes/strings típicas de notas de resgate e spawn suspeito de processsos.
  - Teste essas regras em um SIEM/ELK em ambiente isolado.

- **Playbook IR** (`playbooks/incident_response.md`):
  - Procedimentos recomendados: identificação, contenção, coleta de evidências, erradicação, recuperação e lições aprendidas.

---

## Como executar (apenas em laboratório isolado)
1. Crie uma VM isolada (VirtualBox/VMware) sem acesso irrestrito à rede — preferencialmente host-only ou offline.  
2. Faça snapshot antes de qualquer teste.  
3. Copie os scripts `simulations/` para a VM e verifique os conteúdos antes de executar.  
4. No terminal da VM, execute (exemplo):
   ```bash
   python3 simulations/generate_test_files.py
   python3 simulations/generate_ransom_note.py
Observe os logs/alertas no seu SIEM ou ferramentas (Sysmon/ELK/Velociraptor).

Ao terminar, restaure o snapshot ou remova a VM conforme política do laboratório.

NÃO execute esses scripts em máquinas de produção, em computadores de terceiros, ou em redes sem autorização escrita.

Pasta images/ — prints e evidências visuais
A pasta images/ contém capturas de tela demonstrando:

Execução do generate_test_files.py mostrando a criação dos ficheiros.

Conteúdo do NOTA_RESGATE.txt gerado pelo generate_ransom_note.py.

Dashboards / alertas no ELK / SIEM após execução (ex.: alerta de Mass File Creation).

Exemplos de saída de ferramentas de análise (Process Monitor, Sysmon eventos).

Estrutura recomendada dentro de images/:

markdown
Copiar código
images/
  - test_files_creation_01.png
  - ransom_note_created_01.png
  - siem_alert_mass_file_creation.png
  - sysmon_events_example.png
  - playbook_example_flow.png
Cada imagem tem legenda no relatório docs/report.md explicando o que é e que etapa ela ilustra.

Recomendações de segurança e ética
Sempre obtenha autorização por escrito antes de testar em ambientes que não são de sua propriedade.

Nunca execute testes em ambientes de produção.

Mantenha backups e snapshots atualizados.

Destrua amostras sensíveis e snapshots depois dos testes, ou armazene-os de forma segura e documentada.

Mantenha registros (logs, evidências) de todas as ações realizadas no laboratório.

Como contribuir / adaptar
Se quiser adaptar este repositório:

Sugestões de detecção e thresholds são bem-vindas (abrir issue).

Envie PRs com melhorias nas regras Sigma/YARA e com comentários sobre falsos positivos.

Se acrescentar scripts, documente claramente o propósito e as garantias de segurança.

Licença
Escolha e coloque aqui a licença do projeto (ex.: MIT, CC-BY-NC, etc.). Exemplo:

nginx
Copiar código
MIT License
Contato
Se quiser que eu adapte este README aos nomes reais de arquivos do seu repositório (por exemplo, se você usou nomes diferentes dos citados), diga o nome dos ficheiros e eu atualizo o README imediatamente.
