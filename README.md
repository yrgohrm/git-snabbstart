# Grundläggande Git

## Introduktion

Git är det ledande versionshanteringssystemet idag och är ett väldigt kompetent verktyg. Med den kompetensen kommer dock en stor komplexitet. Den här guiden strävar efter att ge en mycket kort och enkel introduktion till Git och en variant på hur man kan arbeta för att göra flödet så smidigt som möjligt. Den här guiden strävar inte efter att vara heltäckande eller ta upp alla spännande specialfall man kan hamna i.

En mycket grundlig genomgång får man i [Git from the bottom up](https://jwiegley.github.io/git-from-the-bottom-up/)

## Installation

Installation sker olika beroende på vilket OS man har men du hittar den mesta informationen på [https://git-scm.com/downloads](https://git-scm.com/downloads)

## Efter installation

Efter installation är det några saker som kan vara lämpliga att ställa in som standard i git. Kör man windows kommer man enklast åt git genom att starta Git Bash.

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --global pull.rebase true
```

Den sista parametern är omtvistad men gör att man får en rimlig historik när man är flera som arbetar i samma gren och man behöver göra en `git pull` medan man arbetar.

## Välja molnleverantör

Det finns mängder av olika siter som erbjuder hostade Git-repos men följande är tre stora:

- GitHub - [github.com](https://github.com)
- Azure Repos (del av Dev Ops) - [azure.microsoft.com/en-us/services/devops/repos/](https://azure.microsoft.com/en-us/services/devops/repos/)
- GitLab - [gitlab.com](http://gitlab.com/)

Exakt vilka tjänster som ingår är olika, men gällande själva hanteringen av Git fungerar de väldigt lika. Den största skillnaden är antalet användare som kan jobba med privata repos med gratisnivåerna. GitHub tillåter max 4 personer som samarbetar, Azure tillåter 5 personer medan GitLab inte verkar ha någon gräns.

## Arbetsflöde

Nedan är ett enkelt sätt att arbeta som gör att det blir rimligt lätt att göra komplexa förändringar samtidigt som man håller sin huvudgren (master) stabil.

### Skapa repository

Använd den valda leverantörens webbinterface och skapa ett nytt repository. Välj att initiera med en README så du får något från början.

Att skapa ett repository heter att skapa ett projekt på GitLab, medan ett projekt är något annat i både Azure och GitHub.

### Hämta ut repository

Använd kommandot `git clone` för att hämta ut ditt nyskapade repo till din dator.

Använder du Mac eller Linux vill du sannolikt sätta upp ssh-nycklar och klona via ssh istället för via https för att undvika att behöva ange lösenord hela tiden. Du kan självklart göra det för Windows också om du så önskar. Läs t.ex. [GitHubs hjälp om SSH](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) för mer information. Det fungerar väldigt likt i de andra tjänsterna.

```
$ git clone https://github.com/yrgohrm/gittest.git
```

### Flöde för införande av ny funktion

Så fort någon vill lägga till ny funktionalitet till koden skall man göra detta i en gren (branch). Flera personer kan jobba tillsammans i en gren men jobba aldrig direkt mot master.

Ett bra arbetsflöde är som följer:

1. Gör en branch via webbinterfacet. Använd ett bra namn.
2. Kör `git fetch` för att hämta information om den till din dator
3. Kör `git checkout` _`<branch-name>`_
4. Ändra och lägg till i din kod.
5. Kör `git add` och `git commit` för att lägga till din kod lokalt.
6. Kör `git push` för att skicka den till servern. Ev. måste du säga till git var servern finns med `git push --set-upstream origin` _`<branch-name>`_
7. När de modifieringar som skulle göras är färdiga skapa en Pull Request i webbinterfacet. Det heter Merge Request i GitLab.
8. Någon (helst någon annan) granskar koden i grenen genom att titta på och testa koden. Om det förekommer merge-fel gör följande:
   1. `git fetch origin`
   2. `git checkout` _`<branch-name>`_
   3. `git merge origin/master`
   4. Om merge-konflikter uppstår fixa dem och committa in med `git commit -a` Skicka sedan upp dem till servern med `git push`.
9. Acceptera förfrågan och merga ihop grenen. Använder ni Azure välj "Merge (no fast-forward)". Normalt vill man även ta bort grenen när man är klar.

## Ändra och lägg till i din kod

Steg nummer fyra är det som kanske känns mest spännande. När du ändrar din kod behöver du ha koll på tre saker. Det som kallas för "staging area", "commits" och det som är på servern.

En commit är i grunden bara en namngiven uppsättning ändringar och finns endast lokalt på din dator tills de skickas upp till servern med push.

Ditt staging area är där du väljer vilka ändringar som du gjort som skall ingå i en viss commit. Man kan göra detta väldigt komplicerat eller hålla sig till några enkla regler.

Nya filer som inte funnits med tidigare måste läggas till för att de skall komma med i en commit.

`git add` _`<somefile>`_

Filer som redan har varit med i en tidigare commit kan man få med på två olika sätt. Antingen lägger man till dem manuellt precis som för nya filer, eller så använder man flaggan `-a` när man kör `git commit`. Den flaggan säger till git att automatiskt lägga till alla ändrade filer till den aktuella commiten.

En anledning att lägga till de ändrade filerna manuellt är att man då kan köra kommandot `git diff --cached` för att tydligt se alla ändringar innan man committar dem.

För att spara en uppsättning ändringar kör `git commit -m 'meddelande` (eventuellt då även med flaggan `-a`). Där 'meddelande' skall vara ett bra meddelande som handlar om vad ändringarna innebär. Anger du inte `-m` kommer du istället få upp en editor där du kan skriva ett lite längre meddelande.

Jobbar man flera i samma gren kan det vara lämpligt att hämta in ändringar som de skickat till servern till dig ibland för att säkerställa att koden fungerar som det skall. För att hämta nya ändringar kör `git pull` (eller `git pull --rebase` om du inte satte flaggan som kör rebase automatiskt). Du får inte ha ändringar som du inte committat när du gör en pull.

Det finns extremt mycket mer att lära sig om Git, men med det här kommer man i vart fall igång.
