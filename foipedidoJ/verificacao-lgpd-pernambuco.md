# Verificação de privacidade e normas — Central Saúde UPE

Data: 07/11/2026. Escopo: inspeção estática do ZIP e da tela de indicadores, testes locais com dados sintéticos e consulta às fontes abaixo. Não houve auditoria do ambiente de produção nem verificação de todas as normas sanitárias, de regulação de filas ou de todos os atos estaduais. A conclusão não é uma certificação jurídica.

**Resultado: adequação parcial demonstrada; conformidade integral para dados reais não comprovada.**

## Referências e limites

O [repositório indicado pelo usuário](https://github.com/ctresb/leis) foi consultado como ponto de partida. Seu README avisa que conversões automáticas/OCR não substituem a publicação oficial. As tentativas de leitura das pastas e normas individuais falharam (cache, acesso e TLS). Portanto não atribuí ao acervo uma validação normativa que não consegui realizar.

A [LGPD atualizada, disponibilizada pela Câmara](https://www2.camara.leg.br/legin/fed/lei/2018/lei-13709-14-agosto-2018-787077-normaatualizada-pl.html) fundamenta a análise dos controles e pendências abaixo. É preciso definir a base legal para dados sensíveis, atender os direitos do titular e demonstrar segurança e responsabilização. Consentimento não é a única base legal disponível para saúde e políticas públicas. Essas escolhas cabem ao controlador institucional, considerando a operação concreta.

A [SCGE](https://www.scge.pe.gov.br/lgpd-rede-de-encarregados/) identifica o Decreto 49.265/2020 como política estadual e orienta o exercício dos direitos pela Ouvidoria. Também relaciona as Leis 16.420/2018, 14.804/2012, 15.529/2015 e a política de segurança do Decreto 49.914/2020. Esta consulta não confirmou integralmente todos esses textos ou suas alterações; a matriz abaixo indica o que está comprovado e o que depende da instituição.

A [Lei estadual 14.804/2012, art. 17, no portal da SEFAZ](https://www.sefaz.pe.gov.br/Legislacao/Tributaria/Documents/legislacao/Leis_Tributarias/2012/Lei14804_2012.htm) protege informações pessoais e restringe o acesso às relativas à intimidade. O prazo máximo de restrição de acesso ali previsto não é autorização genérica para armazenar todos os dados por cem anos. Indicadores internos não devem ser publicados automaticamente sob argumento de transparência.

## Evidências e ações

| Tema | Evidência no projeto | Conclusão / ação |
|---|---|---|
| Minimização | Endpoint de indicadores retorna contagens, razões, espera e riscos agregados; tela não consulta listas de pacientes | Controle presente no escopo examinado |
| Papéis e escopo | Backend exige GESTOR/ADMIN, aplica escopo às consultas e responde 404 para filtro não autorizado; frontend impede atendente de montar a tela | Controle presente; não substitui validação de produção |
| Rastreabilidade | Rota registra CONSULTAR_INDICADORES com ator e filtros, sem respostas clínicas | Controle presente por inspeção do código |
| Sessão e armazenamento | Transporte existente usa cookies, CSRF, cancelamento e épocas de sessão; nova tela usa memória e URL apenas com datas/UUID | Controle reaproveitado; armazenamento vazio nas conferências visuais |
| Reidentificação | Recortes por fila e tempo podem produzir grupos pequenos | Avaliar risco, acesso, cruzamento e eventual supressão no backend; agregação não garante anonimização |
| Base legal | data-protection.md descreve AGENDAMENTO como consentimento obrigatório | Validar necessidade e base por finalidade com UPE; não alterar unilateralmente o fundamento institucional |
| Retenção | Prazos de solicitações encerradas e auditoria ainda a definir | Aprovar tabela de temporalidade, descarte e conservação legal, incluindo backups |
| Direitos | Documento afirma revogação, mas exportação/exclusão ficam como evolução; implementação do fluxo completo não foi comprovada | Definir canal institucional e atendimento de acesso, correção, revogação e demais direitos, com exceções legais; não exige necessariamente um botão para cada direito |
| Governança estadual | Não foi comprovado vínculo formal desta aplicação ao encarregado, política local e Ouvidoria | Validar com encarregado da UPE e documentar responsabilidades, operadores, inventário e canal |
| Incidentes | incident-response.md declara procedimento apenas para protótipo | Adaptar procedimento para dados reais, responsáveis, avaliação e comunicação conforme regras vigentes |
| Implantação | HTTPS, backups e demais controles dependem do ambiente; PLAN registra pendências | Validar no destino; não inferir implementação a partir do arquivo de configuração |

O protótipo deve continuar usando somente dados sintéticos. Antes de uso real, a UPE precisa decidir os itens institucionais, avaliar impacto e riscos e aprovar a implantação. Não foi alterado backend, regra de fila, política de retenção ou termo de consentimento nesta entrega.
