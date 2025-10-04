#🔐 Projeto — Ransomware & Keylogger (foco em defesa)

---

##📌 Descrição

---

Este repositório documenta simulações seguras e educativas de comportamentos associados a ransomware e keyloggers, com foco em análise e defesa. Nenhum script aqui altera ficheiros fora do escopo do laboratório ou executa captura furtiva de teclas. Execute apenas em VMs isoladas e com autorização.

---

##🛠️ Ambiente Utilizado

Visual Studio Code - (Para criação de scripts)

E-mail do Google - (Para recebimento de e-mails contendo log com saída do keylogger)

---

##✅ Cenários / Scripts (simulações seguras)


Geração de “nota de resgate” e criptografia de arquivos

ransoware.py — criptografa arquivos e cria LEIA_ISSO.txt com mensagem fictícia.

python3 ransoware.py

Descriptografia dos arquivos

descriptografar.py

---

##🔍 Resultados

Prints das execuções, logs e dashboards estão na pasta images/.

---

##🛡️ Recomendações de Mitigação

Manter backups offline e testados.

Habilitar EDR/antivírus com heurísticas comportamentais.

Configurar bloqueio/alerta para criação massiva de ficheiros.

Aplicar princípio do menor privilégio e MFA.

Treinar usuários para reduzir risco de phishing.

---

##⚠️ Aviso Legal / Uso

Use este material somente em ambientes controlados e com permissão. Não execute testes em produção ou em sistemas de terceiros.
