# 00 - Guia do Projeto

## 1. O que é este projeto?

Este repositório documenta um laboratório de estudo para simular uma infraestrutura de TI corporativa pequena e controlada, com foco em identidade, segurança, acesso a recursos e gestão operacional.

A empresa fictícia associada ao laboratório é a HomeLab Consultoria & Contabilidade, uma organização que necessita de um ambiente com utilizadores, departamentos, partilhas de ficheiros, políticas de segurança e controlo de acesso.

O objetivo não é criar um sistema produtivo real, mas sim demonstrar como uma infraestrutura empresarial pode ser organizada e administrada de forma coerente.

---

## 2. Como foi pensado

O projeto foi construído em camadas, para manter a lógica simples e fácil de compreender:

1. Rede base
   - Definir a topologia e os serviços básicos de rede.

2. Identidade e organização
   - Criar um domínio Active Directory com utilizadores, grupos e OUs.

3. Autorização
   - Separar identidade de acesso, usando grupos globais e grupos domain local.

4. Recursos
   - Integrar serviços como ficheiros e partilhas com controlo de permissões.

5. Política e segurança
   - Aplicar GPOs e boas práticas para hardening e controlo administrativo.

6. Evolução
   - Expandir a infraestrutura com funcionalidades mais avançadas, como impressão, LAPS, AD Recycle Bin e monitorização.

Esta abordagem ajuda a evitar misturar três conceitos diferentes:

- quem é o utilizador;
- onde o utilizador pertence;
- quais permissões esse utilizador tem sobre um recurso.

---

## 3. Como ele funciona

O laboratório funciona como um modelo simples de infraestrutra corporativa:

### 3.1 Rede

- O ambiente opera numa rede isolada na sub-rede `10.0.10.0/24`.
- O pfSense atua como gateway, firewall e DHCP.
- O Domain Controller fornece DNS e autenticação interna.

### 3.2 Identidade

- O domínio principal é `lan.homelab.ao`.
- O Active Directory organiza utilizadores, computadores e contas de serviço.
- As OUs definem a estrutura lógica e o contexto para políticas.

### 3.3 Autorização

O modelo de acesso é baseado em grupos:

- Utilizadores pertencem a grupos globais (GG).
- Os grupos globais são associados a grupos domain local (DL).
- Os DL aplicam permissões sobre recursos.

Este modelo segue a lógica:

```text
Utilizador → GG → DL → Recurso
```

### 3.4 Recursos

- O TrueNAS SCALE funciona como servidor de ficheiros.
- As partilhas SMB e os ACLs permitem controlo de acesso por departamento.
- As GPOs aplicam configurações automáticas aos computadores e utilizadores.

### 3.5 Segurança

- O modelo é orientado para o princípio do menor privilégio.
- A gestão administrativa é separada da utilização normal.
- O ambiente evolui para incluir políticas mais rigorosas de segurança.

---

## 4. Como navegar este repositório

Se a ideia for entender o projeto sem se perder, a leitura ideal é esta:

1. [README.md](../README.md) — resumo rápido do projeto.
2. [docs/01-project-overview.md](01-project-overview.md) — contexto e objetivo.
3. [docs/02-architecture.md](02-architecture.md) — visão geral da arquitetura.
4. [docs/03-network.md](03-network.md) — rede e topologia.
5. [docs/03-network/ipam.md](03-network/ipam.md) — plano de endereçamento e atribuições.
6. [docs/04-active-directory.md](04-active-directory.md) — Active Directory e modelo lógico.
7. [docs/08-file-services-design.md](08-file-services-design.md) — desenho dos serviços de ficheiros.
8. [docs/14-truenas-scale-preparacao-integracao-ad.md](14-truenas-scale-preparacao-integracao-ad.md) — implementação do TrueNAS.
9. [docs/13-roadmap.md](13-roadmap.md) — evolução prevista do laboratório.

---

## 5. Estado atual do projeto

O projeto está num estado de laboratório de estudo e documentação, com:

- base de rede e domínio bem definidas;
- Active Directory e estrutura lógica documentadas;
- file services com integração AD já descritos;
- segurança avançada, automação e evolução futura ainda em aberto.

Em termos práticos, o projeto já mostra o desenho da infraestrutura, mas ainda está a evoluir para se tornar mais completo e operacional.

---

## 6. Objetivo final desejado

O objetivo final deste projeto é criar uma referência prática e coerente de uma infraestrutura corporativa pequena, com foco em:

- rede segura e organizada;
- identidade centralizada no Active Directory;
- controlo de acesso por grupos e permissões;
- serviços de ficheiros operacionais;
- políticas de segurança aplicadas via GPO;
- uma estrutura documental que permita compreender e evoluir o laboratório sem perder a consistência.

Para isso, o projeto deve chegar a um estado em que seja possível responder, de forma clara, a estas perguntas:

- O que é este laboratório?
- Como está organizado?
- Que componentes o constituem?
- Como funciona a lógica de identidade e autorização?
- Como evoluir para a próxima etapa sem perder a coerência?

Em termos simples, quer-se chegar a um laboratório que seja:

- fácil de compreender;
- fácil de explicar;
- fácil de evoluir;
- e suficientemente bem documentado para servir de base de estudo e demonstração.

---

## 7. Estado desejado do projeto

O projeto considera-se num estado desejado quando:

- a arquitetura geral está bem explicada e compreensível;
- a topologia da rede está clara;
- o domínio Active Directory está bem definido;
- o modelo de grupos e permissões está consistente;
- os serviços de ficheiros e a integração AD estão documentados de forma objetiva;
- o roadmap distingue claramente entre o que está concluído e o que é evolução futura;
- a documentação permite a alguém novo entender o projeto sem precisar de reconstruir tudo do zero.

Neste ponto, o laboratório deixa de ser apenas uma coleção de ideias e passa a ser uma referência estruturada de estudo.

---

## 8. O que este projeto não é

Este repositório não é:

- uma aplicação de software;
- um ambiente de produção;
- uma implementação automatizada pronta a usar sem adaptação;
- um projeto com todas as peças já finalizadas.

É antes uma referência de aprendizagem e documentação de um laboratório corporativo em construção.
