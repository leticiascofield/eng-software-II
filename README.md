# **Bus Factor** focado em metadados

## Membros do Grupo
- Ana Luisa Messias Ferreira Mendes
- Davi Porto Araujo
- Laura Oliveira Ribeiro
- Leticia Scofield Lenzoni

<br>

## Explicação do sistema

Vamos desenvolver uma ferramenta de **linha de comando (CLI)** para estimar o risco de **monopólio de conhecimento** por arquivo (“bus factor” baixo) usando **metadados** de atividade: commits, autores, arquivos alterados e **pull requests**.

**Seleção de repositórios:** utilizaremos o **SEART GitHub Search Tool (GHS)** para montar a amostra com estes critérios fixos:

* mais de **10 estrelas**
* pelo menos **300 commits**
* pelo menos **15 colaboradores**

**Funcionamento:**

1. **Coleta (Git):** dentro de uma janela temporal de **90 dias**, levantar por arquivo a atividade recente: quem alterou, quantas vezes alterou e quantas **linhas adicionadas/removidas**.
2. **Coleta (PRs):** obter, por autor, a **proporção de PRs aceitas** e demais indicadores de participação no fluxo de revisão.
3. **Cálculo do bus factor por arquivo:** identificar o **autor dominante** e medir sua participação sob **dois ângulos** — **por quantidade de commits** e **por volume de mudanças** (linhas). O arquivo é marcado como **de risco** quando a dominância do autor **ultrapassa 50%** em pelo menos um desses ângulos.
4. **Relatórios:** gerar **tabela no terminal** e exportação **JSON/CSV** listando arquivos de risco com autor dominante, nível de dominância, atividade no período e indicadores de PRs do autor.
5. **Escopo da análise:** focaremos o diretório **`src/`** e **ignoraremos `tests/`** para reduzir ruído de alterações de teste.

**Parâmetros do sistema (valores definidos):**

* **Janela temporal:** 90 dias
* **Limiar de dominância:** 50%
* **Métrica de dominância:** commits **e** linhas
* **Filtros de caminho:** incluir `src/` e excluir `tests/`

**Cuidados:** renomeações/movimentações de arquivos podem afetar o histórico; a análise de PRs está sujeita às políticas de revisão e limites da API do GitHub; o foco é atividade e autoria, não análise semântica do código.

<br>

## Explicação das possíveis tecnologias utilizadas

* **Seleção de repositórios:**

  * **SEART GitHub Search Tool (GHS)** — aplicação dos filtros (estrelas, commits, colaboradores) e seleção dos projetos.
* **Coleta de metadados:**

  * **PyDriller** — varrer o histórico Git local para extrair commits, arquivos modificados e linhas adicionadas/removidas.
  * **GitHub API (REST/GraphQL)** com **PyGithub** — obter dados de pull requests e metadados complementares.
  * **GitPython** — suporte a operações Git locais.
* **CLI e relatórios:**

  * **Typer** — criação da interface de linha de comando e documentação de parâmetros.
  * **Rich** — formatação de tabelas no terminal.
  * **pandas** — agregações e exportação em **JSON/CSV**.
