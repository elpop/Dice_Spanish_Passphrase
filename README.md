# Spanish/English passphrase generator

## Update Notice

* Mon Apr 28 2025, the table name change his name from **dictionary** to **dice_passphrase** for use on PostgreSQL DB as well. you must [copy again the db](#Copy-the-passphrase-db) or [re-create](#DB-Generation) it.

* Sat Apr 26 2025, i change the schema of the sqlite db to permit support new future languages. If you have a DB prior this date, you must [copy again the db](#Copy-the-passphrase-db) or [re-create](#DB-Generation) it.

## Description

Based on the idea from Arnold G. Reinhold ([https://theworld.com/~reinhold/diceware.html](https://theworld.com/~reinhold/diceware.html)), to choose random words ussing a dice to create a really random passphrase.

I create a list of words (in spanish) with the values of the roll of a six faces dice. The vaules are form 1 to 6 and represent the index of a word in the list. The file contains 46,656 words representing 6 values of six dice rolls.

The spanish language has about 107,920 words, but if i put 7 values, we have more posible index than words (279,936 vs 107,920 spanish words). Then i adjust and choose 46,656 to match with the 6 digits index, but use two files to bring 93,312 possible words to the passphrase. odd and even words use the first and second list in alternate order.

The english words are more than spanish, about 370,105 words. I reduce to a seven dice index files (46,656 words/file) to create a corpus of 326,592 posible words.

For example:

```
212114 cabalista
624346 sesquióxido
655221 urticación
216425 cañinque
124665 alegadora
426415 inconcluso
```

Each number in the index, is the result of a dice roll, to obtain the index you must roll the dice six times peer word, if you want a four word passphrase, you need to roll the dice 24 times.

But i make a little program (passphrase.pl) to emulate the dice roll and generate the passphrase.

## Summary

```
$ passphrase.pl -help
Usage:
    passphrase.pl [options]

Options:
    -language or -l
            The -language or -l option show the passphrase on the given
            language:

                passphrase.pl -language es

                or

                passphrase.pl -l es

            Example:

                $ passphrase.pl -l es
                allanabarrancos sochantre melgar prensero

            The values can be "es" for espanish, "en" for english or
            "special" to generate special random char.

            The default value is "es".

    -words or -w
            generate the passphrase with number of words:

                passphrase.pl -language en -words 5

                or

                passphrase.pl -l en -w 5

            Example:

                $ passphrase.pl -l en -w 5
                interpolation stylelessness pussycats tythed typhlocele

            The default value on spanish and english languages is 4.

    -times or -t
            Generate multiple passphrase

                passphrase.pl -language en -words 5 -times 3

                or

                passphrase.pl -l en -w 5 -t 3

            Example:

                $ passphrase.pl -l en -w 5 -t 3
                stinkstone doeskins colorcaster stir chiricahua
                overbulk doughtiness myoedema dallyingly inequidistant
                lollops defrayers carcinomas experimentor fligger

            The Default is 1 and the max value is 1,000.

    -verbose or -v
            show the generation and word selection process:

                passphrase.pl -language en -words 3 -times 2 -verbose

                or

                passphrase.pl -l en -w 3 -t 2 -v

            Example:

                $ passphrase.pl -l en -w 3 -t 2 -v
                Page Index  Word
                  5  656323 verricule           
                  2  534155 rightish            
                  7  431225 nonpoet             
                verricule rightish nonpoet 
                Page Index  Word
                  7  122164 ancylopod           
                  5  442464 overdeal            
                  4  353135 landlordly          
                ancylopod overdeal landlordly

    -help or -h or -?
            Show this help
```

## Install

1. #### Download file

    ```
    git clone https://github.com/elpop/Dice_Passphrase.git
    ```

2. #### Install DB
   
   ##### Sqlite

   The programs use SQLite. This is available for Mac OS and the most popular Linux distros.

    for Debian/Ubuntu Linux systems:

    ```
    sudo apt-get install sqlite3 libsqlite3-dev
    ```

    Fedora/Red-Hat Linux systems:

    ```
    sudo dnf install sqlite sqlite-devel
    ```

    Mac OS

    SQLite is available by default.

    ##### PostgreSQL
    
    Follow the install instructions of the [postgresql.org](https://www.postgresql.org/download/) page.
    
3. #### Perl Dependencies

    [DBI](https://metacpan.org/pod/DBI)

    [DBD::SQLite](https://metacpan.org/pod/DBD::SQLite)

    [DBD::Pg](https://metacpan.org/pod/DBD::Pg) if you use PostgreSQL database engine.

    [Math::Random::Secure](https://metacpan.org/pod/Math::Random::Secure)
    
    [Getopt::Long](https://metacpan.org/pod/Getopt::Long)
    
    [Pod::Usage](https://metacpan.org/pod/Pod::Usage)


    All the Perl Modules are available via [metacpan](https://metacpan.org) or install them via the "cpan" program in your system. Debian/Ubuntu and Fedora have packages for the required perl modules.

    ##### For Fedora/Redhat:

    ```
    sudo dnf install perl-DBI perl-DBD-SQLite perl-Math-Random-Secure perl-Getopt-Long perl-Pod-Usage
    ```
    
    If you use PostgreSQL:
    
    ```
    sudo dnf install perl-DBD-Pg
    ```

    ##### For Debian/Ubuntu:

    ```
    sudo apt-get install libdbi-perl libdbd-sqlite3-perl libmath-random-secure-perl
    sudo cpan -i Getopt::Long Pod::Usage
    ```

    If you use PostgreSQL:
    
    ```
    sudo apt-get install libdbd-pg-perl
    ```

    ##### On Mac OS:

    To compile some Perl modules, you need to install the
    Xcode Command Line Tools:

    ```
    xcode-select --install
    ```

    Install with CPAN:

    ```
    sudo cpan -i DBI DBD::SQLite Math::Random::Secure Getopt::Long Pod::Usage
    ```
    
    If you use PostgreSQL:
    
    ```
    sudo cpan -i DBD::Pg
    ```
    
4. #### Put it on your search path

    Copy the passphrase.pl program somewhere in your search path:
    
    ##### Sqlite

    ```
    sudo cp passphrase.pl /usr/local/bin/.
    ```
    
    ##### PostgreSQL

    The program **passphrase\_postgresql.pl** has 3 parameter you need to edit to match your DB access credentias:
    
    ```
    # Postgres SQL connection parms
    my $db_name = "dbi:Pg:dbname=DB_NAME;host=127.0.0.1";
    my $db_user = "DB_USER";
    my $db_pass = "DB_PASS";
    ```
    
    Then:

    ```
    sudo cp passphrase_postgresql.pl /usr/local/bin/passphrase.pl
    ```

5. #### Copy the passphrase db

    ##### Sqlite
    
    ```
    cd db
    gzip -d passphrase.db.gz
    mkdir ~/.passphrase
    cp passphrase.db ~/.passpharse/.
    ```
    
    ##### PostgreSQL
    
    ```
    cd db
    zcat dice_passphrase.sql.gz | psql YOUR_DB YOUR_DB_USER
    ```

Now, you can use the program :)

## DB Generation

### Sqlite

In case you want to do it. The **db\_load\_words.pl** program create a hidden directory ".passphrase" in your HOME path.

Into the directory create the sqlite DB called "passphrase.db".

when you run it for the first time you see the following:

```
$ rm -rf ~/.passphrase

$ ./db_load_words.pl 
Init DB
load es_words_1.txt...
es_words_1.txt loaded!
load es_words_2.txt...
es_words_2.txt loaded!
load special_chars.txt...
special_chars.txt loaded!
load en_words_1.txt...
en_words_1.txt loaded!
load en_words_2.txt...
en_words_2.txt loaded!
load en_words_3.txt...
en_words_3.txt loaded!
load en_words_4.txt...
en_words_4.txt loaded!
load en_words_5.txt...
en_words_5.txt loaded!
load en_words_6.txt...
en_words_6.txt loaded!
load en_words_7.txt...
en_words_7.txt loaded!
DB ready for use
```

The directory **words** must be in the same path of the **db\_load\_words.pl** program.

The full load of the data can take about 17 minutes or less, depending your computer and disk.

### PostgreSQL

The **db\_load\_word\_postgresql.pl** program create table wit name "dice_passphrase" in your database.

The program has 3 parameter you need to edit to match your DB access credentias:

```
# Postgres SQL connection parms
my $db_name = "dbi:Pg:dbname=DB_NAME;host=127.0.0.1";
my $db_user = "DB_USER";
my $db_pass = "DB_PASS";
```

Then, run the **db\_load\_word\_postgresql.pl** program.

## Words reference

The source of the Spanish words come from [https://github.com/JorgeDuenasLerin/diccionario-espanol-txt](https://github.com/JorgeDuenasLerin/diccionario-espanol-txt)

The file **spanish_words.txt** has the original 107,920 words for your future use o for  create a random subset.

The source of the English words come from [https://github.com/dwyl/english-words](https://github.com/dwyl/english-words)

The file **english_words** has 370,105 words.

## Author

   Fernando Romo <pop@cofradia.org>

## License

```
GNU GENERAL PUBLIC LICENSE Version 3
https://www.gnu.org/licenses/gpl-3.0.en.html
See LICENSE.txt
```

## Sponsor the project

Please [sponsor this project](https://github.com/sponsors/elpop) to pay my high debt on credit cards :)
