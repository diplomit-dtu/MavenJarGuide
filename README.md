# Guide: Byg _executable_ .jar-fil med Maven 
___Forfatter: Malte B. P.___  
_Senest opdateret 07/09-2020_
<br>
<br>

Dette er en kort guide til at bygge en _executable_ .jar-fil med Maven.

_Hvad er en executable .jar-fil?_  
En .jar fil er Java-filer der er kompileret, og samlet til én fil (.jar-filen). Denne fil kan bruges som:
 - et bibliotek til andre Java-programmer
 - et eksekverbart program (executable jar)

<br>

__Vær opmærksom på__:
 - Det forventes at du bruger IntelliJ IDEA
 - Det forventes at du ved hvordan man laver et Maven-projekt i IntelliJ
 - Guiden er lavet ud fra `IntelliJ IDEA 2020.2.1 (Ultimate Edition)`, samt JDK 14, men det burde også virker på lavere versioner
 - Dette .git-repo er et eksempel på resultatet fra guiden, og kan klones og direkte afprøves på din egen computer 


<br>

## Guide

__1. Lav et Maven-projekt i IntelliJ eller benyt ét du allerede har lavet__

__2. Opdatér din pom.xml:__  
 - Kopiér følgende ind i din `pom.xml`-fil:
   ```xml
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <!-- Full name of class containing main function to run -->
                            <mainClass>mypackage.Main</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>

                    <!-- Optional: Stops 'jar-with-dependencies' to be appended the .jar's name -->
                    <appendAssemblyId>false</appendAssemblyId>

                    <!-- Optional: Sets a custom name for the jar file -->
                    <!-- <finalName>MyCustomFileName</finalName> -->
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
    ```
    

- __Husk__ at opdater dit Maven efterfølgende (højreklik på `pom.xml` og tryk `Maven -> Reload project`)

    
__3. Tilføj din "Main"-klasse:__

 - Ret værdien i elementet `<mainClass>mypackage.Main<mainClass>` i din `pom.xml` fra `mypackage.Main` til det fulde navn på klassen, der indeholder den `main`-metode du gerne vil køre, når jar-fil skal køres.  

   > Det fulde navn består af klassen (og dermed filens) navn, samt navnet på pakken, som klassen ligger i. Hvis f.eks. din klasse hedder `Program.java`, og ligger i mappen `src/main/java/mypackage/myotherpackage/Program.java` er det fulde navn `mypackage.myotherpackage.Program`, og der skal derfor stå `<mainClass>mypackage.myotherpackage.Program<mainClass>`



__4. Husk at reload dit Maven-projekt:__
 - Højre klik på `pom.xml` og tryk `Maven -> Reload project`
 

__5. Byg dit Maven-projekt:__

 - Åben Maven-vinduet ved at trykke `View -> Tool Windows -> Maven` i toolbar'en i toppen af IntelliJ
 
 - I det åbnede vindue gå ind under `Lifecycle` og dobbelt-klik på `package` - din .jar-fil bliver nu bygget til `target` mappen i dit projekt!

__6. Done!__

 - Din .jar fil kan køres som en helt almindeligt jar-fil (f.eks. fra terminalen).

<br>__Detaljer__

 - .jar-filen bliver ***ikke bygget*** når du kører dit projekt som normalt (dvs. ved at trykke på den grønne pil) 

 - Din .jar-fil navngives efter dit `<artifactId>` samt `<version>` i `pom.xml`. Alternativt kan du fjerne kommentaren `<finalName>` og sætte dit navn dér.

 