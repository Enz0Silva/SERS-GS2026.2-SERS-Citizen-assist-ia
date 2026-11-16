# Roteiro do teste com usuários (PCP + Energias)

Uma rodada só serve para as duas disciplinas. Meta: **3 ou 4 pessoas de fora do grupo, 10 atendimentos ou mais no total**. Não anote nome, idade, telefone, endereço nem nada que identifique a pessoa: use P1, P2, P3, P4.

## Antes de começar (5 min)

1. Apague o histórico antigo: deixe `data/historico.json` com `[]`.
2. Rode `python main.py` num terminal com letra grande.
3. Explique à pessoa, com estas palavras: "É um assistente de teste sobre conta de luz atrasada e Tarifa Social. Ele não é canal oficial. Não digite CPF, telefone nem e-mail. Pode escrever do seu jeito."
4. Não ajude a pessoa a escolher as palavras. Só anote.

## Tarefas (dê uma de cada vez, em papel ou falando)

| # | Tarefa para a pessoa | Por onde ela deve chegar | Conta para |
|---|---|---|---|
| 1 | "Descubra se uma família do CadÚnico tem direito a desconto na conta de luz." | Pergunta livre | PCP |
| 2 | "Sua conta atrasou. Descubra se podem cortar a luz e com quanto tempo de aviso." | Pergunta livre | PCP |
| 3 | "Descubra se dá para parcelar a conta atrasada." | Pergunta livre ou categorias | PCP |
| 4 | "Pagou a conta e a luz não voltou. Descubra o prazo para religar." | Pergunta livre | PCP |
| 5 | "Use o menu para achar uma orientação sobre a dívida do antigo morador." | Navegar por categorias | PCP |
| 6 | "Descubra o que dá para fazer para gastar menos energia sem gastar dinheiro." | Opção 5 (energia) | PCP e Energias |
| 7 | "Calcule quanto sua casa economizaria com banhos mais curtos." (a pessoa inventa os minutos, não precisa ser a casa dela) | Opção 5, calculadora | PCP e Energias |
| 8 | "Vale a pena colocar placa solar para quem está com a conta atrasada?" | Opção 5, orientação EN03 | Energias |

Com 3 pessoas fazendo 4 tarefas cada, já são 12 atendimentos. Sorteie as tarefas para que todas sejam feitas por pelo menos uma pessoa.

## Depois de cada tarefa de energia, pergunte e anote

- "De 1 a 5, quanto você entendeu a resposta?" (compreensão)
- "De 1 a 5, quanto isso seria útil para uma família com a conta atrasada?" (utilidade)
- "O que ficou confuso ou faltou?" (comentário, nas palavras da pessoa, sem dado pessoal)

Registre essas respostas no Notebook 03 do GS-SERS, seção 7.2, com `registrar_teste_usuario(...)`, uma chamada por tarefa, logo depois do teste. O campo `participante_anonimo` é P1, P2... e cada pessoa tem o seu código.

## Ao sair do assistente

A pessoa dá a nota de facilidade (1 a 5), e o programa exporta o CSV. Troque de pessoa e repita.

## Depois da rodada

1. Abra a opção 3 (indicadores) e copie os números para a seção "Resultado do teste com usuários" do README do PCP.
2. Escolha **uma melhoria** que saiu do teste (palavra-chave nova, texto mais claro) e faça. **Não use "minha luz foi desligada"**: essa fica para a alteração ao vivo do vídeo.
3. Faça commit de `data/historico.json` e `data/atendimentos.csv`.
4. No GS-SERS, rode as seções 7.3 e 8.4 do Notebook 03 e faça commit do `registro_testes_usuarios.csv`.
5. Escreva no README do GS-SERS os comentários recebidos e o ajuste feito.

## Folha de anotação (copie uma por pessoa)

```
Participante: P__        Data: __/11/2026

Tarefa | Achou? (s/n) | Tentativas | Ajudou? (s/n) | Compreensão 1-5 | Utilidade 1-5 | Comentário
-------|--------------|------------|---------------|-----------------|---------------|-----------
       |              |            |               |                 |               |
       |              |            |               |                 |               |
       |              |            |               |                 |               |
       |              |            |               |                 |               |

Nota de facilidade ao sair (1-5): __
Palavras que a pessoa usou e o assistente não entendeu:
```
