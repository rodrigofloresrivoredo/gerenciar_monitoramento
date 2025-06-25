#DESAFIO 2 | gerenciar o monitoramento de recursos MC Azure


Objetivo:  resumos, anotações sobre o uso da Azure, servindo como material de apoio para estudos e futuras implementações.

Recursos - Dos componentes do Azure Monitor
•	Coleta de dados: métricas (numéricas, em série temporal) e logs (texto, JSON, eventos) 
•	Análise:
o	Metrics Explorer para análise visual de métricas
o	Log Analytics com linguagem Kusto (KQL) 
•	Alertas: detecção automatizada com regras e ação automática 
•	Integrações: API, Event Hubs, Logic Apps, Functions, Power BI, Grafana, Insights específicos (VM, Containers) 
Componentes Principais
•	Activity Log (plano de controle)
•	Resource Logs (diagnóstico de recursos) 
•	Workspaces Log Analytics: centralizam logs, permitem consultas KQL, 
•	Insights: Application, VM, Container, etc.
•	Metric Alerts vs Log Alerts:
o	Metric: quase tempo real, estado, dimensões 
o	Log: mais flexível, escalável, mas mais lento e com custo de ingestão 
________________________________________
Configurando o Azure Monitor
Habilitação
•	Ativado automaticamente em nova assinatura, coleta métricas e Activity Log 
•	Para logs detalhados, crie Diagnostic Settings nos recursos 
Configuração de Diagnostic Settings
•	Selecione categorias de logs e destinos:
o	Log Analytics, Storage Account, Event Hubs, serviços parceiros 
________________________________________
Configurando Alertas
Tipos de Alertas
•	Alert Rule: condições baseadas em métrica ou consulta de logs
•	Alert Processing Rule: pós-processamento de alertas
•	Action Group: agrupamento de notificações (email, SMS, e outros) 
Criar Grupos de Ação
•	Defina ação (e-mail, runbook, Logic App)
•	Agrupe em Action Groups e utilize nas regras de alerta 
Criar Regras de Alerta
•	No portal: Monitor → Alertas → Criar → Alert rule
•	Configure:
o	Scope (recursos/cliente)
o	Condition (métrica > threshold ou KQL para logs)
o	Actions (usar Action Group)
o	Details: nome, severidade 
Gerenciar Alertas
•	Visualizar alertas disparados, histórico, estado
•	Usar Alert Processing Rules para automatização ou exclusão condicional
________________________________________
Análise de Logs
Workspaces do Log Analytics
•	Criação: Monitor → Log Analytics workspaces → Criar
•	Escolha assinatura, resource group, região, nível de retenção 
Ingestão de Logs
•	Usando agente AMA + Data Collection Rules (DCR)
•	Logs: Eventos (Windows Event Logs), Syslog, performance counters 
Consultas e KQL
•	Editor Log Analytics permite consultas; há scripts prontos
•	Sintaxe: Heartbeat | where TimeGenerated > ago(1d) etc. 
•	Use funções como summarize, render, extend, bin, joins 
Consultar Dados
•	Via portal Monitor → Logs
•	Ou via API/SDK
•	Usos comuns: análise de performance, erros, disponibilidade, segurança, criação de dashboards, Power BI, e outros.
________________________________________
Métricas Extraídas de Logs
Definir Métricas de Logs
•	Use Scheduled Query Rules API para extrair métrica de logs
•	Criar regra programada, gerar métrica customizada 
Métricas vs Logs
•	Métricas: séries temporais leves → alertas em tempo real
•	Logs: análises detalhadas retroativas via KQL 
________________________________________
Tipos de Dados e Eventos
Tipos de Dados
•	Activity Logs: operações no plano de controle 
•	Resource Logs: operações dentro de cada recurso 
•	Diagnostic Logs / Custom Logs: dados específicos de aplicações ou sistema 
Eventos de Log de Atividades
•	Eventos capturados automaticamente, possíveis de roteamento para Log Analytics
•	Ideal para auditoria, governança, análise histórica 
Identificar Tipos de Dados
•	Use o esquema comum com campos padrão + campos específicos por serviço 
•	Ex.: tabela Events, Heartbeat, Perf, AzureActivity, etc.
________________________________________
Usos do Log Analytics
Diagnóstico e Solução de Problemas
•	Cross-correlacionar logs, buscar causas, fazer root cause analysis 
Segurança e Auditoria
•	Auditoria de ações (Azure AD, recursos), resposta a incidentes
Governança e Compliance
•	Monitorar alterações no ambiente usando Change Analysis 
Observabilidade de Aplicações
•	Application Insights integrado ao Log Analytics
________________________________________
Remissivas para Configuração e Consultas
•	Portal: Monitor → Logs / Métricas para uso direto
•	Azure CLI / PowerShell: scripting de Diagnostic Settings, criação de workspace, alerts
•	API REST / SDK: automação e integração avançada
•	Integrações: Logic Apps, Functions, Power BI, Grafana

Tutorial onde localizar cada item, passo a passo
1.	Como Configurar o Azure Monitor
Etapas:
1.	Acesse o portal do Azure.
2.	Vá em "Monitor" no menu lateral esquerdo.
3.	A partir daí, você já acessa:
o	Logs de atividades
o	Métricas
o	Alertas
o	Workbooks
o	Configurações de diagnóstico
4.	Para habilitar a coleta de logs:
o	Vá até o recurso que deseja monitorar (por exemplo, uma VM).
o	Acesse "Configurações de Diagnóstico".
o	Clique em "Adicionar configuração de diagnóstico".
o	Escolha os tipos de logs e métricas que deseja enviar.
o	Escolha o destino (Log Analytics, Storage, Event Hubs).
________________________________________
2. Como Configurar Alertas no Azure Monitor
Etapas:
1.	Vá em Monitor > Alertas > Criar > Regra de alerta.
2.	Escolha o "Escopo" (ex: uma VM ou App Service).
3.	Defina a condição:
o	Para métrica: ex. CPU > 90%
o	Para log: escreva uma consulta KQL (ex: erros nos últimos 5 minutos)
4.	Selecione um Grupo de Ação (ou crie um novo).
5.	Configure os detalhes: nome da regra, severidade (0 a 4).
6.	Clique em Criar Alerta.
________________________________________
3. Criar Grupos de Ação
Etapas:
1.	Vá em Monitor > Alertas > Grupos de Ação > Novo grupo de ação.
2.	Dê um nome e selecione o resource group.
3.	Em "Notificações", adicione métodos como:
o	E-mail/SMS
o	Webhook
o	Azure Function ou Logic App
4.	Em "Ações", defina o que será executado quando o alerta for disparado.
5.	Salve e utilize este grupo nas regras de alerta.
________________________________________
4. Análise de Logs com Log Analytics
Criar Workspace:
1.	Vá em Log Analytics Workspaces > Criar.
2.	Escolha assinatura, grupo de recursos, nome e região.
3.	Finalize a criação.
Configurar Diagnóstico:
1.	No recurso (como VM ou App Service), vá em "Configurações de diagnóstico".
2.	Envie os logs para o workspace criado.
Consultar Logs:
1.	Vá em Monitor > Logs.
2.	Escolha o workspace.
3.	Use consultas KQL (Kusto Query Language). Exemplo:
kql
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count() by Computer
________________________________________
5. Definir Métricas com Base em Logs
Como fazer:
1.	Vá em Monitor > Alertas > Regras de alerta programada (Scheduled Query Rule).
2.	Use uma consulta KQL para extrair a condição do log.
3.	Se desejar, envie o resultado para criar uma métrica personalizada.
________________________________________
6. Tipos de Dados e Eventos
Tipos principais:
•	Activity Logs: ações administrativas no portal (quem criou, deletou, etc.)
•	Resource Logs: operações internas dos recursos (ex: conexões recusadas)
•	Métricas: dados numéricos agregados em tempo real
•	Custom Logs: logs definidos por você
________________________________________
7. Usos do Log Analytics
•	Diagnóstico de VMs, Apps, redes e containers
•	Alertas baseados em logs (ex: login com falha)
•	Visualizações com Workbooks
•	Integração com Power BI
•	Segurança com Azure Defender / Sentinel
