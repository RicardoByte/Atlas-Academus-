___
## 🏋 Sistema de Gestão de Academia - Atlas Academus

---

## 1. Resumo do Negócio

  

O Sistema de Gestão de Academia é uma solução desenvolvida para automatizar os processos administrativos, operacionais e de acompanhamento físico de academias.

  

O sistema centraliza informações sobre alunos, instrutores, planos, treinos, equipamentos e pagamentos, permitindo um controle eficiente e integrado.

  

**O fluxo operacional é linear e intuitivo:**

  

> O aluno se matricula → escolhe um plano → recebe uma avaliação → o instrutor cria uma ficha de treino → o aluno realiza os exercícios.

  

Esse modelo facilita o gerenciamento e melhora a experiência tanto do aluno quanto do administrador da academia.

  

---
## 2. Objetivos do Negócio

  

- Modernizar a gestão de academias com um sistema integrado

- Controlar matrículas, planos e pagamentos de forma automatizada

- Gerenciar o relacionamento entre alunos e instrutores

- Registrar e acompanhar o desempenho físico dos alunos

- Otimizar a alocação de equipamentos e a manutenção do espaço

  

---
## 3. Problematização

  

A administração manual de academias frequentemente resulta em perda de informações, atrasos em pagamentos e dificuldade de controle de treinos.

  

Com o aumento da demanda por eficiência e digitalização no setor fitness, um sistema de gestão torna-se essencial para manter a competitividade e a organização do negócio.

  

---
## 4. Público-Alvo

  

- Academias de pequeno e médio porte

- Estúdios de personal trainer

- Profissionais autônomos que gerenciam treinos e avaliações

  

---
## 5. Estrutura Funcional do Sistema

  

O sistema foi projetado com base em **10 tabelas principais** que representam as áreas fundamentais da operação da academia.

| Entidade | Função no Negócio |

|----------|-------------------|

| **Pessoa** | Armazena dados gerais de todas as pessoas cadastradas no sistema. |

| **Aluno** | Representa pessoas matriculadas na academia; especialização de Pessoa. |

| **Instrutor** | Representa pessoas responsáveis pelos treinos; especialização de Pessoa. |

| **Plano** | Define os tipos de adesão (Mensal, Trimestral, Anual) e seus valores. |

| **Matrícula** | Registra a adesão do aluno a um plano e o período de vigência. |

| **Treino** | Organiza os programas de exercícios criados pelo instrutor. |

| **Exercício** | Armazena os diferentes exercícios disponíveis no sistema. |

| **FichaDeTreino** | Relaciona os treinos e exercícios atribuídos a cada aluno. |

| **Equipamento** | Controla os equipamentos utilizados e suas manutenções. |

| **Avaliação Física** | Registra medições corporais e informações sobre o desempenho físico. |

  

---
## 6. Especializações

  

### Pessoa

Pode se especializar em:

- **Aluno** (realiza matrícula e treinos)

- **Instrutor** (cria treinos e realiza avaliações)

  

### Plano

Possui três categorias:

- Mensal

- Trimestral

- Anual

  

---
## 7. Funcionamento do Sistema (Fluxo do Negócio)

  

1. **Cadastro de Pessoa**: O usuário insere informações básicas de identificação e contato

2. **Especialização**: Essa pessoa é classificada como Aluno ou Instrutor

3. **Definição de Plano**: O aluno escolhe um plano (Mensal, Trimestral ou Anual)

4. **Matrícula**: É criada uma matrícula vinculando o aluno ao plano escolhido

5. **Avaliação Física**: O instrutor realiza uma avaliação inicial do aluno

6. **Criação do Treino**: O instrutor elabora um treino e registra os exercícios correspondentes

7. **Ficha de Treino**: O treino é associado ao aluno através da ficha de treino

8. **Execução**: O aluno realiza os exercícios de acordo com a ficha

9. **Controle de Equipamentos**: O sistema gerencia o uso e manutenção dos equipamentos

10. **Pagamentos**: O sistema registra e controla os pagamentos de matrículas e planos

  

---
## 8. Vantagens Competitivas

  

- ✅ Redução de tarefas manuais e erros administrativos

- ✅ Controle total de cadastros, planos e pagamentos

- ✅ Histórico completo de avaliações e treinos

- ✅ Comunicação simplificada entre instrutores e alunos

- ✅ Relatórios de desempenho físico e financeiro

  

---
## 9. Viabilidade e Sustentabilidade

  

O investimento inicial envolve o desenvolvimento e implantação do sistema.

  

Após implementado, os custos de manutenção são baixos e os ganhos operacionais são significativos — redução de inadimplência, maior controle de alunos ativos e otimização do tempo dos instrutores.

  

Além disso, o sistema pode ser comercializado como **Software as a Service (SaaS)**, permitindo que outras academias utilizem a mesma solução mediante assinatura mensal.

  

---
## 10. Conclusão

  

O Sistema de Gestão de Academia oferece uma estrutura tecnológica eficiente para gerenciar todas as operações de uma academia de forma integrada.

  

Com base em **10 tabelas bem definidas** e especializações claras, o sistema garante um fluxo linear, fácil de compreender e manter.

  

Esse modelo representa uma oportunidade de modernização, controle e crescimento sustentável para o negócio fitness.