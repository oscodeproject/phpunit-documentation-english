

.. _appendixes.annotations:

===========
Annotatie
===========

Een annotatie is een speciale vorm van syntactische metadata die kan worden 
toegevoegd aan de broncode van sommige programmeertalen. Terwijl PHP geen 
speciale taalfunctie heeft voor het annoteren van broncode, is het gebruik 
van tags zoals de ``@annotation arguments`` in een documentatie blok gecreëerd
door de PHP community om broncode te annoteren. In PHP zijn documentatie
blokken reflecterend: ze zijn toegankelijk op functie, class, methode en 
attributen niveau via de Reflection API's ``getDocComment()`` methode.
Applicaties zoals PHPUnit gebruiken deze informatie tijdens runtime om hun
gedrag te configureren. 


.. admonition:: Note

   Een documentatie opmerking in PHP moet beginnen met ``/**`` en eindigen met
   ``*/``. Annotaties met een andere opmerking stijl worden genegeerd.

Deze bijlage toont alle variaties van annotaties die ondersteund worden door PHPUnit.

.. _appendixes.annotations.author:

@author
#######

De ``@author`` annotatie is een alias voor de
``@group`` annotatie (zie :ref:`appendixes.annotations.group`) en staat toe om testen te filteren gebaseerd op hun auteurs.

.. _appendixes.annotations.after:

@after
######

De ``@after`` annotatie kan worden toegepast voor het specificeren van methodes die aangeroepen dienen te worden na iedere test methode in een test case class.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @after
         */
        public function tearDownSomeFixtures(): void
        {
            // ...
        }

        /**
         * @after
         */
        public function tearDownSomeOtherFixtures(): void
        {
            // ...
        }
    }

.. _appendixes.annotations.afterClass:

@afterClass
###########

De ``@afterClass`` annotatie kan worden toegepast voor het specificeren van static methodes die aangeroepen dienen te worden nadat alle test methodes in een test class zijn uitgevoerd om de gedeelde fixtures op te ruimen.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @afterClass
         */
        public static function tearDownSomeSharedFixtures(): void
        {
            // ...
        }

        /**
         * @afterClass
         */
        public static function tearDownSomeOtherSharedFixtures(): void
        {
            // ...
        }
    }

.. _appendixes.annotations.backupGlobals:

@backupGlobals
##############

PHPUnit kan optioneel backup alle globale en super-globale variabele voor iedere test en deze backup terugplaatsen na iedere test.

De ``@backupGlobals enabled`` annotatie kan worden toegepast op class niveau om deze actie te activeren voor alle testen van een test case class:

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @backupGlobals enabled
     */
    final class MyTest extends TestCase
    {
        // ...
    }

De ``@backupGlobals`` annotatie kan ook worden gebruikt op test methode niveau. Dit maakt verfijnde configuraties mogelijk voor de backup en terugplaats acties:

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @backupGlobals enabled
     */
    final class MyTest extends TestCase
    {
        public function testThatInteractsWithGlobalVariables()
        {
            // ...
        }

        /**
         * @backupGlobals disabled
         */
        public function testThatDoesNotInteractWithGlobalVariables(): void
        {
            // ...
        }
    }

.. _appendixes.annotations.backupStaticAttributes:

@backupStaticAttributes
#######################

PHPUnit kan optioneel backup alle static attributen in alle gedeclareerde classes voor iedere test en de backup terugplaatsen na iedere test.

De ``@backupStaticAttributes enabled`` annotatie kan worden gebruikt op class niveau om deze actie te activeren voor alle testen van een test case class:

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @backupStaticAttributes enabled
     */
    final class MyTest extends TestCase
    {
        // ...
    }

De ``@backupStaticAttributes`` annotatie kan ook worden toegepast op test methode niveau. Dit maakt verfijnde configuratie mogelijk voor de backup en terugplaats acties:

.. code-block:: php

    use PHPUnit\Framework\TestCase;

    /**
     * @backupStaticAttributes enabled
     */
    class MyTest extends TestCase
    {
        public function testThatInteractsWithStaticAttributes(): void
        {
            // ...
        }

        /**
         * @backupStaticAttributes disabled
         */
        public function testThatDoesNotInteractWithStaticAttributes(): void
        {
            // ...
        }
    }

.. admonition:: Note

   ``@backupStaticAttributes`` is gelimiteerd door PHP internals en er      kunnen mogelijk niet bedoelde static waarden aangehouden en              doorgegeven worden aan opvolgende testen in sommige situaties.

   Zie :ref:`fixtures.global-state` voor details.

.. _appendixes.annotations.before:

@before
#######

De ``@before`` annotatie kan worden toegepast om methodes te specificeren die aangeroepen dienen te worden voor iedere test methode in een test case class.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @before
         */
        public function setupSomeFixtures(): void
        {
            // ...
        }

        /**
         * @before
         */
        public function setupSomeOtherFixtures(): void
        {
            // ...
        }
    }

.. _appendixes.annotations.beforeClass:

@beforeClass
############

De ``@beforeClass`` annotatie kan worden toegepast om static methodes te specificeren die aangeroepen dienen te worden voor iedere test methoden in een test class om gedeelde fixtures op te zetten.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @beforeClass
         */
        public static function setUpSomeSharedFixtures(): void
        {
            // ...
        }

        /**
         * @beforeClass
         */
        public static function setUpSomeOtherSharedFixtures(): void
        {
            // ...
        }
    }

.. _appendixes.annotations.codeCoverageIgnore:

@codeCoverageIgnore*
####################

De ``@codeCoverageIgnore``,
``@codeCoverageIgnoreStart`` en
``@codeCoverageIgnoreEnd`` annotaties kunnen worden gebruikt voor het excluden van code regels uit de dekking 
analyses.

Voor gebruik zie :ref:`code-coverage-analysis.ignoring-code-blocks`.

.. _appendixes.annotations.covers:

@covers
#######

De ``@covers`` annotatie kan worden gebruikt in de test code om te specificeren welke delen van de code het dient te testen:

.. code-block:: php

    /**
     * @covers \BankAccount
     */
    public function testBalanceIsInitiallyZero(): void
    {
        $this->assertSame(0, $this->ba->getBalance());
    }

Indien aanwezig dan wordt de code dekking rapport effectief gefilterd om van de uitgevoerde code alleen de gerefereerde code delen toe te voegen. Dit zorgt ervoor dat code alleen als gedekt gemarkeerd worden als er toegewijde testen voor zijn, maar niet als het indirect wordt gebruikt door testen van een andere class, wat valse positieve van code dekking voorkomt.

Deze annotatie kan worden toegevoegd aan de docblock van een test class of individuele test methodes. De aanbevolen manier is om de annotatie toe te voegen aan de docblock van een test class en niet aan de docblock van de test methodes.

Als de ``forceCoversAnnotation`` configuratie optie in de
:ref:`configuration file <appendixes.configuration>` is gezet op ``true``, dan dient iedere test methode een associatie te hebben met  ``@covers`` annotatie 
(oftewel op de test class of de individuele test methode).

:numref:`appendixes.annotations.covers.tables.annotations` toont de syntax van de ``@covers`` annotatie.
De sectie :ref:`code-coverage-analysis.specifying-covered-parts`
geeft grotere voorbeelden voor het gebruik van de annotatie.

Let op, deze annotatie vereist een fully-qualified class name (FQCN).
Om dit duidelijker te maken aan de lezer is het aanbevolen om leading backslash te gebruiken (ook al is het niet vereist voor het correct werken van de annotatie).

.. rst-class:: table
.. list-table:: Annotaties voor het specificeren welke methodes gedekt zijn met een test.
    :name: appendixes.annotations.covers.tables.annotations
    :header-rows: 1

    * - Annotatie
      - Omschrijving
    * - ``@covers ClassName::methodName`` (niet aanbevolen)
      - Specificeert dat de annotated test methode de specifieke methode dekt.
    * - ``@covers ClassName`` (aanbevolen)
      - Specificeert dat de annotated test methode alle methodes van een gegeven class dekt.
    * - ``@covers ClassName<extended>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode alle methoden van gegeven class en zijn parent class(es) dekt.
    * - ``@covers ClassName::<public>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode alle public methodes van een gegeven class dekt.
    * - ``@covers ClassName::<protected>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode alle protected methodes van een gegeven class dekt.
    * - ``@covers ClassName::<private>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode calle private methodes van een gegeven class dekt.
    * - ``@covers ClassName::<!public>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode alle methoden die niet public zijn van een gegeven class dekt.
    * - ``@covers ClassName::<!protected>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode alle methoden die niet protected zijn van een gegeven class dekt.
    * - ``@covers ClassName::<!private>`` (niet aanbevolen)
      - Specificeert dat de annotated test methode alle methoden die niet private zijn van een gegeven class dekt.
    * - ``@covers ::functionName`` (aanbevolen)
      - Specificeert dat de annotated test methode de gespecificeerde globale functie dekt.

.. _appendixes.annotations.coversDefaultClass:

@coversDefaultClass
###################

De ``@coversDefaultClass`` annotatie kan worden gebruikt om een default namespace of class naam te specificeren. Hierdoor hoeven lange namen niet herhaald te worden voor iedere ``@covers`` annotatie. Zie 
:numref:`appendixes.annotations.examples.CoversDefaultClassTest.php`.

Let op, deze annotatie vereist een fully-qualified class name (FQCN).
Om dit duidelijker te maken aan de lezer is het aanbevolen om leading
backslash te gebruiken (ook al is het niet vereist voor het correct werken van de annotatie).

.. code-block:: php
    :caption: Using @coversDefaultClass to shorten annotations
    :name: appendixes.annotations.examples.CoversDefaultClassTest.php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @coversDefaultClass \Foo\CoveredClass
     */
    final class CoversDefaultClassTest extends TestCase
    {
        /**
         * @covers ::publicMethod
         */
        public function testSomething(): void
        {
            $o = new Foo\CoveredClass;
            $o->publicMethod();
        }
    }

.. _appendixes.annotations.coversNothing:

@coversNothing
##############

De ``@coversNothing`` annotatie kan worden gebruikt in de test code om te specificeren dat er geen code dekking informatie zal worden opgenomen voor de 
annotated test case.

Dit kan worden gebruikt voor integratie testen. Zie 
:ref:`code-coverage-analysis.specifying-covered-parts.examples.GuestbookIntegrationTest.php`
voor een voorbeeld.

De annotatie kan worden gebruikt op class en methode niveau en overschrijft iedere ``@covers`` tags.

.. _appendixes.annotations.dataProvider:

@dataProvider
#############

Een test methode kan arbitrary argumenten accepteren. Deze argumenten dienen te worden geleverd door een of meerdere data provider methodes (``provider()`` in
:ref:`writing-tests-for-phpunit.data-providers.examples.DataTest.php`).
De data provider methode die gebruikt dient te worden, word gespecificeerd door gebruik te maken van de
``@dataProvider`` annotatie.

Zie :ref:`writing-tests-for-phpunit.data-providers` voor meer informatie.

.. _appendixes.annotations.depends:

@depends
########

PHPUnit ondersteund de declaratie van explicit dependencies tussen test 
methodes. Zulke dependencies definieren niet de volgorde van de uitvoering 
van de test methodes maar staan de terugkeer van een instantie van een test 
fixture van een maker en het doorgeven naar de afhankelijke gebruikers toe.
:ref:`writing-tests-for-phpunit.examples.StackTest2.php` toont hoe de 
``@depends`` annotatie gebruikt kan worden om de dependencies tussen test 
methodes aan te geven.

Zie :ref:`writing-tests-for-phpunit.test-dependencies` voor meer informatie.

.. _appendixes.annotations.doesNotPerformAssertions:

@doesNotPerformAssertions
#########################

Voorkomt dat een test die geen assertions uitvoerd als riskant word gezien.

.. _appendixes.annotations.group:

@group
######

Een test kan worden gelabeld dat het behoort tot een of meerdere groepen door de
``@group`` annotatie als volgt te gebruiken

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @group specification
         */
        public function testSomething(): void
        {
        }

        /**
         * @group regression
         * @group bug2204
         */
        public function testSomethingElse(): void
        {
        }
    }

De ``@group`` annotatie kan ook worden toegepast op de test class.
Het is dan "inherited" door alle test methodes van die test class.

Testen kunnen geselecteerd worden voor uitvoering gebaseerd op 
groepen door gebruik te maken van de ``--group`` en ``--exclude-group`` 
opties van de command-line test runner of met gebruikmaking van de 
respectieve richtlijnen van de XML configuratie bestand.

.. _appendixes.annotations.large:

@large
######

De ``@large`` annotatie is een alias voor
``@group large``..

Als de ``PHP_Invoker`` package is geinstaleerd en strict
mode is enabled, dan zal een grote test falen als de uitvoering 
langer dan 60 seconden duurt. Deze timeout is instelbaar via de
``timeoutForLargeTests`` attribuut in de XML
configuratie bestand.

.. _appendixes.annotations.medium:

@medium
#######

The ``@medium`` annotation is an alias for
``@group medium``. A medium test must not depend on a test
marked as ``@large``.

If the ``PHP_Invoker`` package is installed and strict
mode is enabled, a medium test will fail if it takes longer than 10
seconds to execute. This timeout is configurable via the
``timeoutForMediumTests`` attribute in the XML
configuration file.

.. _appendixes.annotations.preserveGlobalState:

@preserveGlobalState
####################

When a test is run in a separate process, PHPUnit will
attempt to preserve the global state from the parent process by
serializing all globals in the parent process and unserializing them
in the child process. This can cause problems if the parent process
contains globals that are not serializable. To fix this, you can prevent
PHPUnit from preserving global state with the
``@preserveGlobalState`` annotation.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @runInSeparateProcess
         * @preserveGlobalState disabled
         */
        public function testInSeparateProcess(): void
        {
            // ...
        }
    }

.. _appendixes.annotations.requires:

@requires
#########

The ``@requires`` annotation can be used to skip tests when common
preconditions, like the PHP Version or installed extensions, are not met.

A complete list of possibilities and examples can be found at
:ref:`incomplete-and-skipped-tests.requires.tables.api`

.. _appendixes.annotations.runTestsInSeparateProcesses:

@runTestsInSeparateProcesses
############################

Indicates that all tests in a test class should be run in a separate
PHP process.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @runTestsInSeparateProcesses
     */
    final class MyTest extends TestCase
    {
        // ...
    }

*Note:* By default, PHPUnit will
attempt to preserve the global state from the parent process by
serializing all globals in the parent process and unserializing them
in the child process. This can cause problems if the parent process
contains globals that are not serializable. See :ref:`appendixes.annotations.preserveGlobalState` for information
on how to fix this.

.. _appendixes.annotations.runInSeparateProcess:

@runInSeparateProcess
#####################

Indicates that a test should be run in a separate PHP process.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class MyTest extends TestCase
    {
        /**
         * @runInSeparateProcess
         */
        public function testInSeparateProcess(): void
        {
            // ...
        }
    }

*Note:* By default, PHPUnit will
attempt to preserve the global state from the parent process by
serializing all globals in the parent process and unserializing them
in the child process. This can cause problems if the parent process
contains globals that are not serializable. See :ref:`appendixes.annotations.preserveGlobalState` for information
on how to fix this.

.. _appendixes.annotations.small:

@small
######

The ``@small`` annotation is an alias for
``@group small``. A small test must not depend on a test
marked as ``@medium`` or ``@large``.

If the ``PHP_Invoker`` package is installed and strict
mode is enabled, a small test will fail if it takes longer than 1
second to execute. This timeout is configurable via the
``timeoutForSmallTests`` attribute in the XML
configuration file.

.. admonition:: Note

   Tests need to be explicitly annotated by either ``@small``,
   ``@medium``, or ``@large`` to enable run time limits.

.. _appendixes.annotations.test:

@test
#####

As an alternative to prefixing your test method names with
``test``, you can use the ``@test``
annotation in a method's DocBlock to mark it as a test method.

.. code-block:: php

    /**
     * @test
     */
    public function initialBalanceShouldBe0(): void
    {
        $this->assertSame(0, $this->ba->getBalance());
    }

.. _appendixes.annotations.testdox:

@testdox
########

Specifies an alternative description used when generating the agile
documentation sentences.

The ``@testdox`` annotation can be applied to both test classes and test methods.

.. code-block:: php

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @testdox A bank account
     */
    final class BankAccountTest extends TestCase
    {
        /**
         * @testdox has an initial balance of zero
         */
        public function balanceIsInitiallyZero(): void
        {
            $this->assertSame(0, $this->ba->getBalance());
        }
    }

.. admonition:: Note

   Prior to PHPUnit 7.0 (due to a bug in the annotation parsing), using
   the ``@testdox`` annotation also activated the behaviour
   of the ``@test`` annotation.

When using the ``@testdox`` annotation at method level with a ``@dataProvider`` you may use the method parameters as placeholders in your alternative description.

.. code-block:: php

    /**
     * @dataProvider additionProvider
     * @testdox Adding $a to $b results in $expected
     */
    public function testAdd($a, $b, $expected)
    {
        $this->assertSame($expected, $a + $b);
    }

    public function additionProvider()
    {
        return [
            [0, 0, 0],
            [0, 1, 1],
            [1, 0, 1],
            [1, 1, 3]
        ];
    }

.. _appendixes.annotations.testWith:

@testWith
#########

Instead of implementing a method for use with ``@dataProvider``,
you can define a data set using the ``@testWith`` annotation.

A data set consists of one or many elements. To define a data set
with multiple elements, define each element in a separate line.
Each element of the data set must be an array defined in JSON.

See :ref:`writing-tests-for-phpunit.data-providers` to learn
more about passing a set of data to a test.

.. code-block:: php

    /**
     * @testWith ["test", 4]
     *           ["longer-string", 13]
     */
    public function testStringLength(string $input, int $expectedLength): void
    {
        $this->assertSame($expectedLength, strlen($input));
    }

An object representation in JSON will be converted into an associative array.

.. code-block:: php

    /**
     * @testWith [{"day": "monday", "conditions": "sunny"}, ["day", "conditions"]]
     */
    public function testArrayKeys(array $array, array $keys): void
    {
        $this->assertSame($keys, array_keys($array));
    }

.. _appendixes.annotations.ticket:

@ticket
#######

The ``@ticket`` annotation is an alias for the
``@group`` annotation (see :ref:`appendixes.annotations.group`) and allows to filter tests based
on their ticket ID.

.. _appendixes.annotations.uses:

@uses
#####

The ``@uses`` annotation specifies code which will be
executed by a test, but is not intended to be covered by the test. A good
example is a value object which is necessary for testing a unit of code.

.. code-block:: php

    /**
     * @covers \BankAccount
     * @uses   \Money
     */
    public function testMoneyCanBeDepositedInAccount(): void
    {
        // ...
    }

:numref:`code-coverage-analysis.specifying-covered-parts.examples.InvoiceTest.php`
shows another example.

In addition to being helpful for persons reading the code,
this annotation is useful in strict coverage mode
where unintentionally covered code will cause a test to fail.
See :ref:`risky-tests.unintentionally-covered-code` for more
information regarding strict coverage mode.

Please note that this annotation requires a fully-qualified class name (FQCN).
To make this more obvious to the reader, it is recommended to use a leading
backslash (even if this is not required for the annotation to work correctly).
