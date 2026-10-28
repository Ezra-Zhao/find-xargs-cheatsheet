# find + xargs Cheatsheet · Folha de referência de comandos find + xargs

> 🌍 [中文](README.md) · [Español](README.es.md)

Uma **folha de referência de uma só página** com 30 usos reais de `find` e `xargs`: procurar arquivos por nome, tipo, tamanho, data e permissões; combinar condições sem cair na armadilha clássica de `-o` com `-print`; agir sobre os resultados com `-exec`, `-execdir` e `-printf`; e entregar a lista ao `xargs` com segurança (`-print0`/`-0` contra nomes com espaços, `-P` em paralelo, `-n` e `-I` para controlar os lotes). Cada entrada responde só a três perguntas: **o que faz, um comando pronto para copiar e colar, e etiquetas de uso**. Sem parâmetros inventados: as 30 entradas foram **executadas de verdade**, uma a uma, numa árvore de testes com GNU findutils 4.9.0, e a semântica foi conferida no manual oficial do GNU Findutils.

## Para quem é

Engenheiros de operações (SRE/DevOps), desenvolvedores backend e qualquer pessoa que, ao escrever um `find`, precise conferir se o `-exec` termina em `\;` ou em `+`, ou que já viu um pipe para o `xargs` quebrar por causa de um nome de arquivo com espaços. Se procurar, renomear ou limpar arquivos em lote te trava, esta folha resolve de uma vez.

## Como usar

1. Vai direto ao **[CHEATSHEET.md](CHEATSHEET.md)**: o corpo da referência; os comandos em inglês qualquer desenvolvedor entende.
2. Você precisa do GNU findutils: vem com o Linux; o macOS traz a versão BSD e algumas opções (como o `-r` do xargs) diferem — a folha marca as diferenças.
3. Segue a ordem da folha —básico → filtros → lógica → ações → xargs— e testa cada comando à mão num diretório de prática. Em uma hora você domina.

## Teste em 30 segundos

```bash
find --version | head -1           # imprime a versão (p. ex. find (GNU findutils) 4.9.0): é a versão GNU
mkdir -p /tmp/play && cd /tmp/play # diretório de prática: ensaie aqui tudo o que é destrutivo
touch app.log notes.txt            # cria dois arquivos e testa a combinação abaixo na hora
```

## A combinação de ouro, pronta para usar

```bash
find . -name "*.txt" -print0 | xargs -0 grep -l "TODO"
```

Imune a espaços, aspas e quebras de linha nos nomes: o find separa com NUL (o único byte que um nome de arquivo nunca pode conter) e o xargs `-0` lê com NUL. Em árvores de diretórios dos outros, use sempre este par.

## O que inclui

| Categoria | Conteúdo |
|---|---|
| 🔍 Básico | -name por nome / -iname ignorando maiúsculas / -type por tipo / -path sobre o caminho completo / -maxdepth e -mindepth (5) |
| 🎯 Filtros | -size por tamanho (com a armadilha do arredondamento) / -mtime e -mmin por data / -newer contra um arquivo de referência / -empty / -perm em três modos / -regex (6) |
| 🧩 Lógica | Parênteses para o -o, negação com !, e a armadilha do -print que se prende ao ramo errado (1) |
| ⚙️ Ações | -exec \; um a um / -exec {} + em lotes / -execdir dentro do diretório / -ok perguntando antes / -printf saída sob medida / -ls listagem / -delete apagar (7) |
| 🚀 xargs | Pipe básico / o par seguro -print0 + -0 / -n limite por leva / -I marcador / -P paralelo / -t eco / -p perguntar / -r não executar se vazio / -d delimitador / -a de arquivo / -E marca de fim (11) |

A documentação oficial ligada é apenas https://www.gnu.org/software/findutils/manual/; todos os comandos foram conferidos no manual do GNU Findutils (capítulos `find` e `xargs`) e executados com sucesso no GNU findutils 4.9.0; os exemplos operam só sobre um diretório de prática criado na hora, sem dados reais.

## Por que estas 30

- **O par seguro, primeiro**: um pipe comum corta os nomes em qualquer espaço, e um nome com espaços se parte em pedaços — `-print0` + `-0` é a combinação para memorizar antes de qualquer outra.
- **`;` e `+` diferem em ordens de grandeza**: `-exec ... \;` lança um processo por arquivo; `-exec ... {} +` enche uma linha de comando antes de executar. Com milhares de arquivos, é a diferença entre minutos e segundos.
- **Duas armadilhas, avisadas antes**: ao juntar uma ação a uma expressão com `-o`, o `-print` se prende só ao último ramo e você perde metade dos resultados em silêncio; e o `-size` arredonda para cima antes de comparar, então `-size -1M` só casa com arquivos vazios. As duas levam ⚠️ e a forma correta na folha.
- **O destrutivo se ensaia**: `-delete` não tem lixeira; `-ok` e `xargs -p` são o modo de ensaio dele — primeiro a lista, depois a ação.

## PRs são bem-vindos

Achou um comando errado, uma opção obsoleta, uma tradução fraca? Pull Requests são bem-vindos. Mantenha o formato das entradas (uma linha de explicação / bloco bash / etiquetas) e **só comandos reais e verificados; na dúvida, deixe de fora**.

## 📚 Mais folhas de referência

Outras folhas do mesmo autor, também em chinês / espanhol / português:

- [Folha de SSH](https://github.com/Ezra-Zhao/ssh-cheatsheet) — 28 comandos de conexão, chaves, túneis e endurecimento
- [Folha de FFmpeg](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 comandos de áudio e vídeo
- [Folha de Git](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 comandos diários e de resgate
- [Folha de Docker](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 comandos de contêineres
- [Folha de curl](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 comandos para depurar APIs
- [Folha de expressões regulares](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 padrões práticos
- [Folha de jq](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 filtros para JSON
- [Folha de superpoderes do terminal](https://github.com/Ezra-Zhao/terminal-superpowers-cheatsheet) — 14 ferramentas de produtividade
- [Folha de kubectl](https://github.com/Ezra-Zhao/kubectl-cheatsheet) — comandos habituais do Kubernetes
- [Folha de SQL](https://github.com/Ezra-Zhao/sql-cheatsheet) — consultas e funções habituais
- [Folha de Vim](https://github.com/Ezra-Zhao/vim-cheatsheet) — 30 comandos de edição, busca e macros
- [Folha de tmux](https://github.com/Ezra-Zhao/tmux-cheatsheet) — 30 comandos de sessões, janelas e painéis
- [Folha de systemd](https://github.com/Ezra-Zhao/systemd-cheatsheet) — 30 comandos de serviços e logs

## Licença

MIT © 2026 Guangyi Zhao. Veja [LICENSE](LICENSE).
