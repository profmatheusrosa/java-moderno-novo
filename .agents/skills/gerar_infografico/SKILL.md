---
name: gerar-infografico
description: Use this skill ALWAYS when the user requests generating technical or educational illustrations, diagrams, or infographics. This skill ensures visual consistency, prevents English words, and enforces strict Brazilian Portuguese (pt-BR) labels.
---

# Gerar Infográfico Educacional (pt-BR)

Esta habilidade define as diretrizes rigorosas e o padrão de prompt para gerar infográficos educacionais com estilo *flat vector* e **alta densidade de informação**, garantindo que o texto presente na imagem seja exclusivamente em Português do Brasil (pt-BR) e que não contenha expressões em inglês.

## Quando usar esta habilidade
Sempre que a tarefa envolver a chamada da ferramenta (tool) `generate_image` para fins de ilustração, diagramas de arquitetura ou explicações visuais de engenharia de software no contexto do projeto.

## Diretrizes de Construção do Prompt

O AI agent (você) deve seguir as seguintes regras ao gerar a imagem:

1. **Idioma do Prompt:** O prompt DEVE ser escrito inteiramente em **Inglês** (para melhor alinhamento estético).
2. **Proibições Estritas:**
   - **NÃO** peça para escrever o nome do idioma ou siglas como "pt-BR" ou "Português" dentro da imagem.
   - **NÃO** permita rótulos ou qualquer palavra em inglês na imagem final.
3. **Densidade Informativa:** O infográfico não deve conter apenas rótulos curtos, mas também **frases descritivas concisas** que expliquem o conceito (ex: "Uma classe deve ter apenas um motivo para mudar" em vez de apenas "Responsabilidade Única").
4. **Layout Detalhado:** Peça por "detailed layout" e "highly informative" para evitar imagens excessivamente simplistas.
5. **Especificação Exata de Rótulos:** Liste *apenas* as opções precisas de rótulos e frases em pt-BR.
6. **Sufixo Estético Obrigatório:** O final do prompt deve sempre conter:
   `Professional design, technical aesthetic, clear and educational, flat vector style, software engineering infographic.`

## Template Oficial do Prompt

Copie e preencha este modelo de prompt cada vez que a ferramenta `generate_image` precisar ser chamada:

```text
A highly detailed and informative educational infographic illustrating [CONCEITO GERAL]. 
[DESCRIÇÃO VISUAL DETALHADA: divisões em colunas/painéis, ícones, estrutura]. 
Include symbols and short descriptive text for each element.
Use ONLY the following words for labels and descriptions: '[Palavra 1]', '[Frase explicando 1]', '[Palavra 2]', '[Frase explicando 2]'. 
DO NOT use English words. DO NOT write 'pt-BR' anywhere in the image. 
Professional design, technical aesthetic, clear and educational, flat vector style, software engineering infographic.
```

## Fluxo de Execução Recomendado

1. **Identificação:** Leia o conceito e identifique os pilares fundamentais que precisam de explicação.
2. **Planejamento de Texto:** Decida os rótulos **e frases curtas** em pt-BR que aparecerão como legenda.
3. **Preenchimento:** Substitua os espaços "[ ]" do **Template Oficial**.
4. **Execução:** Chame `generate_image` com o `ImageName` em formato snake_case.
5. **Cópia:** Emita imediatamente o comando para copiar o arquivo gerado para a pasta `imagens/` do projeto.

