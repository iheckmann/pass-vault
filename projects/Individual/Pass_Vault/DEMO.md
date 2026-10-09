# Pass_Vault: DEMO (Isabel Heckmann)
LINK PARA O VÍDEO:


## O projeto
O Pass_Vault é um gerenciador de senhas de linha de comando em Python. Ele guarda credenciais num arquivo criptografado, protegido por uma **senha mestra**. A senha mestra é transformada em chave com **Argon2id** (um KDF lento de propósito, com salt aleatório), e o arquivo é criptografado com **AES-256-GCM**, que protege o conteúdo e detecta se alguém adulterou o arquivo. As gravações são atômicas, para a queda de energia não corromper o cofre.

A base do projeto já trazia `init`, `add`, `get`, `list`, `delete`, `gen` e `change-password`. Meu trabalho foi estender essa base com os **4 desafios do Nível 1**.

## O que eu implementei
1. **`pv search <texto>`:** lista as entradas cujo nome (ou usuário) contém o texto, sem diferenciar maiúsculas de minúsculas. Busca vazia é rejeitada, para não virar um `list` disfarçado.
2. **`pv count`:** imprime só o número de entradas, com `print()` simples para funcionar em scripts.
3. **Último uso:** novo campo `last_used_at` em cada entrada. O `get` atualiza a data e salva o cofre. Cofres antigos, sem o campo, continuam abrindo.
4. **`pv get --show`:** a senha fica escondida (`********`) por padrão e só aparece com `--show`, útil para compartilhar tela sem expor nada.

## Resultados e validação
- `just test`: **69 testes passando**.
- `just lint`: ruff, mypy e pylint sem erros.
- Cada comando novo foi executado na CLI com um cofre de teste, com dados falsos, em ambiente próprio.

## Conclusões e decisões
- **Camadas:** o `vault.py` define o que é uma entrada e protege os dados; o `main.py` só cuida da interface. Por isso o campo `last_used_at` nasceu no `vault.py`.
- **Trade-off do último uso:** guardar essa data significa que quem roubar o arquivo do cofre (mesmo sem abrir) descobre qual entrada foi usada por último. É um ganho de usabilidade com um pequeno custo de privacidade.
- **Seguro por padrão:** a senha oculta é o comportamento padrão, e mostrar é opt-in.
- **O que eu faria diferente:** estendi a busca para o usuário também, algo que o desafio não pedia; e escreveria testes automatizados específicos para os comandos novos.

## Próximo passo
Fazer os desafios do Nível 2 (export/import seguro, força de senha) e seguir para o projeto em equipe `Secrets`.