APT Slack Spear Phishing Campaign
======

- [Español](#espanol)
  - [Introducción](#introduccion)
  - [Detalles técnicos](#detalles)
  - [IOCs](#indicadores)
  - [Posible atribución](#atribucion)
  - [Referencias](#referencias)
- [English](#english)
  - [Intro](#intro)
  - [Technical Details](#details)
  - [IOCs](#indicators)
  - [Possible Attribution](#attribution)
  - [References](#references)

<a name="espanol"></a>
# Español: Campaña de Spear Phishing de APT dirigida a Slack
<a name="introduccion"></a>
## Introducción

En enero de 2024, observamos por primera vez una campaña de Spear Phishing dirigida a usuarios de Slack en el sector financiero.

El actor de amenazas utilizó una plantilla para el correo engañoso con un alto nivel de atención al detalle, lo que la hace muy convincente. Sin embargo, parte de su infraestructura está reciclada, ya que se ha utilizado previamente en campañas anteriores, y muestra actividad compatible con un Actor de Amenazas Persistente (APT).

Hasta la fecha de esta investigación, no hay menciones públicas de los indicadores de esta campaña, lo que sugiere que estamos entre los primeros en descubrirla y perfilarla [[1]](#referencias).

<a name="detalles"></a>
## Detalles técnicos

El atacante aprovecha servidores de correo electrónico de Google para obtener una ventaja reputacional y eludir posibles bloqueos basados en listas de SPAM.

El cuerpo del correo no varía, pero sí cambian sus asuntos y los enlaces maliciosos del botón de acción. La plantilla utilizada muestra un alto nivel de sofisticación, con especial atención al detalle, incluyendo enlaces y botones que redirigen a sitios y redes sociales oficiales de la aplicación Slack. Sin embargo, el lenguaje empleado en el cuerpo del mensaje y los asuntos de los correos revela la naturaleza engañosa de la comunicación, como se analiza en la sección 'Análisis del lenguaje'.

Los dominios maliciosos utilizados por el actor de amenazas están alojados en PorkBun, al igual que los utilizados por [QRLog](https://github.com/birminghamcyberarms/QRLog/). Al momento de redactar este informe, todos estos dominios muestran un mensaje de "Mantenimiento", como se puede observar a continuación. Esto se debe probablemente a la publicación de los indicadores de compromiso en distintas plataformas de inteligencia (disponibles en la sección 'Referencias').

![Mensaje de mantenimiento](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Mantenimiento.png)

Los dominios fueron creados el 3 de Enero de 2024, excepto por *slack-hub* que existe desde Mayo de 2021 según Kaspersky. Todos tienen reputación neutra o positiva en su plataforma.

![Reputation](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Reputation1.png)

![Reputation](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Reputation2.png)

![Reputation](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Reputation3.png)

### Análisis del lenguaje

#### Cuerpo del mensaje
```
Kindly acknowledge receiving of our deal!
We urge you to immediately address the requirement to accept our recent updated deal through your company‘s messaging platform. Kindly enter your official communication account and promptly acknowledge your business‘s newest deal.
```

- Uso de "receiving of" en lugar de "receipt of".
- Uso de "recent updated" en lugar de "recently updated."
- Uso de "kindly", palabra poco común en comunicaciones normales y muy frecuente en correos engañosos.

#### Asuntos

```
Take Immediate Action: Accept Your Latest Business Agreement on Slack!
Action Needed: Log in and Consent to the Revised Agreement on Communication!
Time-sensitive: Verify Your Company's New Slack Deal Immediately!
Time's Running Out: Recognize Your Recently Company Agreement on Slack!
URGENT: Approval Required for the Fresh Business Contract on Messaging!
Critical Mission: Acknowledge Your Business's Latest Agreement on Communication!
Vital Update: Accept Your Recent Company Pact on Slack!
Crucial: Acknowledge the New Contract on Company's Messaging!
Don't Miss Out on This: Acknowledge Your Company's Newest Agreement on Slack!
Act Now: Accept Your Newest Company Terms on Slack!
VITAL: Approval Required for the New Company Pact on Slack!
Respond Immediately: Validate Your Latest Corporate Agreement on Slack!
Critical Assignment: Validate Your Company's Most Recent Agreement on Slack!
Critical Task: Confirm Your Company's Newest Deal on Slack!
Important Update: Accept Your Most Recent Company Deal on Slack!
PRESSING: Approval Required for the New Company Agreement on Slack!
Urgent: Login and Endorse the Updated Agreement on Slack!
Essential Duty: Authorize Your Company's Recent Contract on Messaging!
```

- Uso de un patrón común: [Frase de alerta]: [necesidad de acción]!.
- Uso de un patrón común que intenta engañar al usuario mediante un falso sentido de urgencia.
- Uso de signos de exclamación innecesarios.
- Uso de palabras y frases poco comunes en comunicaciones corporativas como "Pressing", "Vital", "Respond Immediately", "Crucial".
- Se presume que los asuntos son generados por algún modelo LLM como ChatGPT.

<a name="indicadores"></a>
## IOCs

```
Domain:slackcloud.network
Domain:slack-hub.com
Domain:slack-sso.com
Domain:slack-protect.com
Domain:gfylinks.com
Domain:badtastecru.co.uk
Domain:ssoslack.com
Domain:slack.com.slackcloud.network
Domain:slack.com.ssoslack.com
Domain:com.ssoslack.com
IP:44.227.65.245
IP:44.227.76.166
URL:http://com.ssoslack.com/signin
URL:http://slack.com.ssoslack.com/signin
URL:http://ssoslack.com/signin
URL:https://badtastecru.co.uk/jxp8g
URL:https://gfylinks.com/saa5l
URL:https://slack.com.slackcloud.network/signin#/signin
String:Take Immediate Action: Accept Your Latest Business Agreement on Slack!
String:Action Needed: Log in and Consent to the Revised Agreement on Communication!
String:Time-sensitive: Verify Your Company's New Slack Deal Immediately!
String:Time's Running Out: Recognize Your Recently Company Agreement on Slack!
String:URGENT: Approval Required for the Fresh Business Contract on Messaging!
String:Critical Mission: Acknowledge Your Business's Latest Agreement on Communication!
String:Vital Update: Accept Your Recent Company Pact on Slack!
String:Crucial: Acknowledge the New Contract on Company's Messaging!
String:Don't Miss Out on This: Acknowledge Your Company's Newest Agreement on Slack!
String:Act Now: Accept Your Newest Company Terms on Slack!
String:VITAL: Approval Required for the New Company Pact on Slack!
String:Respond Immediately: Validate Your Latest Corporate Agreement on Slack!
String:Critical Assignment: Validate Your Company's Most Recent Agreement on Slack!
String:Critical Task: Confirm Your Company's Newest Deal on Slack!
String:Important Update: Accept Your Most Recent Company Deal on Slack!
String:PRESSING: Approval Required for the New Company Agreement on Slack!
String:Urgent: Login and Endorse the Updated Agreement on Slack!
String:Essential Duty: Authorize Your Company's Recent Contract on Messaging!
```

<a name="atribucion"></a>
## Posible Atribución

La infraestructura utilizada para esta campaña es reciclada, ya que ha formado parte de campañas anteriores. Si bien los dominios son nuevos, las direcciones IP a las que están vinculados tienen más de 100 menciones en la plataforma OTX de AlienVault [[2]](#referencias) [[3]](#referencias). Tras un analisis con la herramienta [bIOChip](https://github.com/mauroeldritch/bIOChip) (de mi autoría) encontré vínculos con los APT `EVILNUM` (principalmente) y `Lazarus` (en segundo lugar). Recordemos que los dominios utilizados en la campaña de QRLog de Lazarus también estaban alojados en Porkbun, detalle no menor.

```
#Output de bIOChip 

⚠️  Report for concrecapital.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Lazarus (1)
☣️  Domain is linked to malware activity:
	* WifiCloudWidget (1)
	* Targeted: Crypto.com (1)
	* Targeted: Coinbase (1)

⚠️  Report for superimarkets.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* VileLoader (1)
	* DeathStalker (1)
	* Stonefly (1)
	* Maui (1)
	* EVILNUM (1)

⚠️  Report for 1jdm.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* AM (1)
	* Agent Tesla (1)
	* Malware (1)
	* Tulach Malware (1)
	* adware.pcappstore/veryfast (1)
	* NSIS (1)
	* Static AI - Malicious PE (1)
	* HoneyPot (1)

⚠️  Report for azureservicesapi.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* VileLoader (1)
	* DeathStalker (1)
	* Stonefly (1)
	* Maui (1)
	* EVILNUM (1)

⚠️  Report for symantecq.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* VileLoader (1)
	* DeathStalker (1)
	* Stonefly (1)
	* Maui (1)
	* EVILNUM (1)

⚠️  Report for slack-sso.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Evilnum (1)
☣️  Domain is linked to malware activity:
	* EVILNUM (1)

⚠️  Report for slack-hub.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Evilnum (1)
☣️  Domain is linked to malware activity:
	* EVILNUM (1)

⚠️  Report for slack-protect.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Evilnum (1)
☣️  Domain is linked to malware activity:
	* EVILNUM (1)
```

### Sobre EVILNUM

Evilnum, DeathStalker, TA4563 o Knockout Spider es un actor de amenazas avanzado centrado en víctimas del sector financiero y criptomonedas. Fue observado por primera vez en 2017 y continúa activo. Es conocido por emplear el Spear Phishing entre sus TTPs y por utilizar su propio conjunto de herramientas, que incluye PyVil RAT (un troyano escrito en Python) y EVILNUM. También se le ha observado utilizando herramientas de terceros como More_Eggs y herramientas de código abierto como LaZagne.

Tiene vínculos con Golden Chickens, proveedores de servicios MaaS (malware como servicio).

<a name="referencias"></a>
## Referencias
1) [Pulso de inteligencia en AlienVault OTX sobre la campaña](https://otx.alienvault.com/pulse/65a00d8fd54e59676ca9d3c0)
2) [Pulso de inteligencia en AlienVault OTX sobre infraestructura](https://otx.alienvault.com/indicator/ip/44.227.65.245)
3) [Pulso de inteligencia en AlienVault OTX sobre infraestructura](https://otx.alienvault.com/indicator/ip/44.227.76.166)
4) [Perfil de EVILNUM en Crowdstrike Falcon](https://falcon.us-2.crowdstrike.com/intelligence-v2/actors/knockout-spider/summary)
5) [Perfil de EVILNUM en Mitre](https://attack.mitre.org/groups/G0120/)
6) [bIOChip](https://github.com/mauroeldritch/bIOChip)
7) [Herramientas de EVILNUM en Mitre](https://attack.mitre.org/software/S0568/)

--

<a name="english"></a>
# English: APT Spear Phishing Campaign Targeting Slack
<a name="intro"></a>
## Introduction

In January 2024, we first identified a Spear Phishing campaign targeting Slack users in the financial sector.

The threat actor used a template for the deceptive email with a high level of attention to detail, making it highly convincing. However, part of its infrastructure is recycled, having been used in previous campaigns, and it shows activity consistent with an Advanced Persistent Threat (APT).

As of the date of this investigation, there are no public mentions of the indicators of this campaign, suggesting that we are among the first to discover and profile it [[1]](#references).

<a name="details"></a>
## Technical Details

The attacker uses Google email servers to gain a reputational advantage and evade potential blocks based on SPAM lists.

The body of the email does not vary, but its subjects and malicious action button links do change. The template used shows a high level of sophistication, with special attention to detail, including links and buttons redirecting to official Slack application sites and social networks. However, the language used in the email body and the email subjects reveals the deceptive nature of the communication, as analyzed in the 'Language Analysis' section.

Malicious domains used by the threat actor are hosted on PorkBun, similar to those used by [QRLog](https://github.com/birminghamcyberarms/QRLog/). At the time of writing this report, all these domains display a "Maintenance" message, as seen below. This is likely due to the release of compromise indicators on various intelligence platforms (available in the 'References' section).

![Maintenance message](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Mantenimiento.png)

According to Kaspersky, domains were created on January 3rd except for *slack-hub* which exists since May, 2021. All of them have none or positive reputation on their platform.

![Reputation](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Reputation1.png)

![Reputation](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Reputation2.png)

![Reputation](https://raw.githubusercontent.com/MauroEldritch/EVILSLACK/main/img/Reputation3.png)


### Language Analysis

#### Email Body
```
Kindly acknowledge receiving of our deal!
We urge you to immediately address the requirement to accept our recent updated deal through your company‘s messaging platform. Kindly enter your official communication account and promptly acknowledge your business‘s newest deal.
```

- Use of "receiving of" instead of "receipt of."
- Use of "recent updated" instead of "recently updated."
- Use of "kindly," a word uncommon in normal communications and very frequent in deceptive emails.

#### Subjects

```
Take Immediate Action: Accept Your Latest Business Agreement on Slack!
Action Needed: Log in and Consent to the Revised Agreement on Communication!
Time-sensitive: Verify Your Company's New Slack Deal Immediately!
Time's Running Out: Recognize Your Recently Company Agreement on Slack!
URGENT: Approval Required for the Fresh Business Contract on Messaging!
Critical Mission: Acknowledge Your Business's Latest Agreement on Communication!
Vital Update: Accept Your Recent Company Pact on Slack!
Crucial: Acknowledge the New Contract on Company's Messaging!
Don't Miss Out on This: Acknowledge Your Company's Newest Agreement on Slack!
Act Now: Accept Your Newest Company Terms on Slack!
VITAL: Approval Required for the New Company Pact on Slack!
Respond Immediately: Validate Your Latest Corporate Agreement on Slack!
Critical Assignment: Validate Your Company's Most Recent Agreement on Slack!
Critical Task: Confirm Your Company's Newest Deal on Slack!
Important Update: Accept Your Most Recent Company Deal on Slack!
PRESSING: Approval Required for the New Company Agreement on Slack!
Urgent: Login and Endorse the Updated Agreement on Slack!
Essential Duty: Authorize Your Company's Recent Contract on Messaging!
```

- Use of a common pattern: [Alert Phrase]: [Action Needed]!.
- Use of a common pattern attempting to deceive the user through a false sense of urgency.
- Use of unnecessary exclamation marks.
- Use of uncommon words and phrases in corporate communications such as "Pressing," "Vital," "Respond Immediately," "Crucial."
- Presumed generation of subjects by an LLM model like ChatGPT.

<a name="indicators"></a>
## IOCs

```
Domain:slackcloud.network
Domain:slack-hub.com
Domain:slack-sso.com
Domain:slack-protect.com
Domain:gfylinks.com
Domain:badtastecru.co.uk
Domain:ssoslack.com
Domain:slack.com.slackcloud.network
Domain:slack.com.ssoslack.com
Domain:com.ssoslack.com
IP:44.227.65.245
IP:44.227.76.166
URL:http://com.ssoslack.com/signin
URL:http://slack.com.ssoslack.com/signin
URL:http://ssoslack.com/signin
URL:https://badtastecru.co.uk/jxp8g
URL:https://gfylinks.com/saa5l
URL:https://slack.com.slackcloud.network/signin#/signin
String:Take Immediate Action: Accept Your Latest Business Agreement on Slack!
String:Action Needed: Log in and Consent to the Revised Agreement on Communication!
String:Time-sensitive: Verify Your Company's New Slack Deal Immediately!
String:Time's Running Out: Recognize Your Recently Company Agreement on Slack!
String:URGENT: Approval Required for the Fresh Business Contract on Messaging!
String:Critical Mission: Acknowledge Your Business's Latest Agreement on Communication!
String:Vital Update: Accept Your Recent Company Pact on Slack!
String:Crucial: Acknowledge the New Contract on Company's Messaging!
String:Don't Miss Out on This: Acknowledge Your Company's Newest Agreement on Slack!
String:Act Now: Accept Your Newest Company Terms on Slack!
String:VITAL: Approval Required for the New Company Pact on Slack!
String:Respond Immediately: Validate Your Latest Corporate Agreement on Slack!
String:Critical Assignment: Validate Your Company's Most Recent Agreement on Slack!
String:Critical Task: Confirm Your Company's Newest Deal on Slack!
String:Important Update: Accept Your Most Recent Company Deal on Slack!
String:PRESSING: Approval Required for the New Company Agreement on Slack!
String:Urgent: Login and Endorse the Updated Agreement on Slack!
String:Essential Duty: Authorize Your Company's Recent Contract on Messaging!
```

<a name="attribution"></a>
## Possible Attribution

The infrastructure used for this campaign is recycled, having been part of previous campaigns. While the domains are new, the linked IP addresses have more than 100 mentions on the AlienVault OTX platform [[2]](#references) [[3]](#references). After an analysis with the [bIOChip](https://github.com/mauroeldritch/bIOChip) tool, I found links to APT `EVILNUM` (mostly) and `Lazarus` (secondarily). Remember that the domains used in the QRLog campaign by Lazarus were also hosted on Porkbun, a noteworthy detail.

```
#bIOChip Output

⚠️  Report for concrecapital.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Lazarus (1)
☣️  Domain is linked to malware activity:
	* WifiCloudWidget (1)
	* Targeted: Crypto.com (1)
	* Targeted: Coinbase (1)

⚠️  Report for superimarkets.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* VileLoader (1)
	* DeathStalker (1)
	* Stonefly (1)
	* Maui (1)
	* EVILNUM (1)

⚠️  Report for 1jdm.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* AM (1)
	* Agent Tesla (1)
	* Malware (1)
	* Tulach Malware (1)
	* adware.pcappstore/veryfast (1)
	* NSIS (1)
	* Static AI - Malicious PE (1)
	* HoneyPot (1)

⚠️  Report for azureservicesapi.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* VileLoader (1)
	* DeathStalker (1)
	* Stonefly (1)
	* Maui (1)
	* EVILNUM (1)

⚠️  Report for symantecq.com: Malicious activity found.
☣️  Domain is linked to malware activity:
	* VileLoader (1)
	* DeathStalker (1)
	* Stonefly (1)
	* Maui (1)
	* EVILNUM (1)

⚠️  Report for slack-sso.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Evilnum (1)
☣️  Domain is linked to malware activity:
	* EVILNUM (1)

⚠️  Report for slack-hub.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Evilnum (1)
☣️  Domain is linked to malware activity:
	* EVILNUM (1)

⚠️  Report for slack-protect.com: Malicious activity found.
🥷🏻  Domain is linked to known adversaries:
	* Evilnum (1)
☣️  Domain is linked to malware activity:
	* EVILNUM (1)
```

### About EVILNUM

Evilnum, DeathStalker, TA4563, or Knockout Spider is an advanced threat actor focused on victims in the financial sector and cryptocurrencies. First observed in 2017, it remains active. It is known for employing Spear Phishing among its TTPs and using its toolset, including PyVil RAT (a Python-written Trojan) and EVILNUM. It has also been observed using third-party tools like More_Eggs and open-source tools like LaZagne.

It has links with Golden Chickens, providers of MaaS (malware as a service).

<a name="references"></a>
## References
1) [AlienVault OTX Intelligence Pulse on the campaign](https://otx.alienvault.com/pulse/65a00d8fd54e59676ca9d3c0)
2) [AlienVault OTX Intelligence Pulse on infrastructure](https://otx.alienvault.com/indicator/ip/44.227.65.245)
3) [AlienVault OTX Intelligence Pulse on infrastructure](https://otx.alienvault.com/indicator/ip/44.227.76.166)
4) [CrowdStrike Falcon profile of EVILNUM](https://falcon.us-2.crowdstrike.com/intelligence-v2/actors/knockout-spider/summary)
5) [EVILNUM profile on Mitre](https://attack.mitre.org/groups/G0120/)
6) [bIOChip](https://github.com/mauroeldritch/bIOChip)
7) [EVILNUM Toolset on Mitre](https://attack.mitre.org/software/S0568/)