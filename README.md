<h1>Introduction</h1>
<p>
This jQuery plugin enables complex culture-aware number and date parsing and formatting, including the raw culture information for hunderds of different languages and countries, as well as an extensible system for localization.
</p>
<ul>
<li><a href="#why">Why Globalization</a></li>
<li><a href="#what">What is a Culture?</a></li>
<li><a href="#cultures">jQuery.cultures</a></li>
<li><a href="#culture">jQuery.culture</a></li>
<li><a href="#prefer">jQuery.preferCulture</a></li>
<li><a href="#find">jQuery.findClosestCulture</a></li>
<li><a href="#format">jQuery.format</a></li>
<li><a href="#parseInt">jQuery.parseInt</a></li>
<li><a href="#parseFloat">jQuery.parseFloat</a></li>
<li><a href="#parseDate">jQuery.parseDate</a></li>
<li><a href="#localize">jQuery.localize</a></li>
<li><a href="#extend">Utilizing and Extending Cultures</a></li>
<li><a href="#defining">Defining Culture Information</a></li>
<li><a href="#numbers">Number Formatting</a></li>
<li><a href="#dates">Date Formatting</a></li>
</ul>

<a name="why"></a>
<h2 id="why">Why Globalization?</h2>
<p>
Each language, and the countries that speak that language, have different expectations when it comes to how numbers (including currency and percentages) and dates should appear. Obviously, each language has different names for the days of the week and the months of the year. But they also have different expectations to the structure of dates, such what order the day, month and year are in. In number formatting, not only does the character used to deliniate number groupings and the decimal portion, but the placement of those characters differ. 
</p>
<p>
A user using an application should be able to read and write dates and numbers in the format they are occustomed to. This plugin makes this possible, providing an API to convert user-entered numbers and dates -- in their own format -- into real numbers and dates, and conversely, to format numbers and dates into that format.
</p>

<h2 id="what">What is a Culture?</h2>
<p>
jQuery.glob.js defines roughly 350 cultures. Part of the reason for this large number, besides there being a lot of cultures of the world, is because for some languages, expectations differ amoung the countries that speak it. English, for example, is an official language in dozens of countries. Despite the language being English, the expected date formatting still greatly differs between them.
</p>
<p>
So, it seems it is not useful to define cultures by their language alone. Nor is it useful to define a culture by it's country alone, as many countries have many official languages, spoken by sizable populations. Therefore, cultures are defined as a combination of the language and the country speaking it. Each culture is given a unique code that is a combination of an ISO 639 two-letter lowercase culture code for the language, and a two-letter uppercase code for the country or region. For example, "en-US" is the culture code for English, in the United States.
</p>
<p>
Yet, it is perhaps unreasonable to expect application developers to cater to every possible language/country combination perfectly. It is important then to define so-called "neutral" cultures based on each language. This culture defines the most likely accepted set of rules by anyone speaking that language, whatever the country. Neutral cultures are defined only by their language code. For example, "es" is the neutral culture for Spanish.
</p>

<h2 id="cultures">jQuery.cultures</h2>
<p>
A mapping of culture code to that culture. For example, jQuery.cultures.fr is an object representing the complete culture definition for the neutral French culture. Note that jQuery.glob.js alone only includes an English culture. To get additional cultures, you must seperately reference one or more of the culture scripts that come with it. See below on what fields are defined in each culture.
</p>

<h2 id="culture">jQuery.culture</h2>
<p>
This is set to the culture currently being used. This serves as the default culture if no culture is specified on the various parsing and formatting functions. For example, to change the current culture, set it to one of the available cultures:
<pre>
jQuery.culture = jQuery.cultures["FR-fr"];
</pre>
</p>

<h2 id="prefer">jQuery.preferCulture</h2>
<p>
An application that supports globalization and/or localization will need to have a way to determine the user's preference. Attempting to automatically determine the appropriate culture is useful, but it is good practice to always offer the user a choice, by whatever means. 
</p>
<p>
Whatever your mechanism, it is likely you would have to correlate the user's preferences with the list of cultures the app has specifically decided to support. This API allows you to tell jQuery.glob.js what cultures the user prefers, and select the best match given the culture scripts you have included.
</p>
<p>If you pass an array, the first culture for which there is a match (that culture's script has been referenced) will be used. If none match, the search restarts using the corresponding neutral cultures. For example, if the application has included only the neutral "fr" culture, any of these would select it:
<pre>
&lt;script src="jQuery.glob.fr.js" type="text/javascript">&lt;/script>
...
jQuery.preferCulture("fr");
alert(jQuery.culture.name) // 'fr'

jQuery.preferCulture("fr-FR");
alert(jQuery.culture.name) // 'fr'

jQuery.preferCulture(["es-MX", "fr-FR"]);
alert(jQuery.culture.name) // 'fr'
</pre>

In any case, if no match is found, the invariant culture is selected.
</p>

<h2 id="find">jQuery.findClosestCulture</h2>
<p>
Just like preferCulture, but it returns the matching culture, if any, rather than setting jQuery.culture to it.
</p>

<h2 id="format">jQuery.format(value, format, culture)</h2>
<p>
Formats a date or number according to the given format string and the given culture (or the current culture if not specified). See the section below on number and date formatting tokens for details on the available formats.
<pre>
// assuming a culture with number grouping of 3 digits, using "," separator and "." decimal symbol.
jQuery.format(1234.567, "n"); // "1,234.57"
jQuery.format(1234.567, "n1"); // "1,234.6"
jQuery.format(1234.567, "n0"); // "1,235"

// assuming a culture with "/" as the date separator symbol
jQuery.format(new Date(1955,10,5), "yyyy/MM/dd"); // 1955/11/05
jQuery.format(new Date(1955,10,5), "dddd MMMM d, yyyy"); // Saturday November 5, 1955
</pre>
</p>

<h2 id="parseInt">jQuery.parseInt(value, radix, culture)</h2>
<p>
Parses a string representing a whole number in the given the radix (10 by default), taking into account any formatting rules followed by the given culture (or the current culture, if not specified). 
<pre>
// assuming a culture where "," is the group separator and "." is the decimal separator
jQuery.parseInt("1,234.56"); // 1234
// assuming a culture where "." is the group separator and "," is the decimal separator
jQuery.parseInt("1.234,56"); // 1234
</pre>
</p>

<h2 id="parseFloat">jQuery.parseFloat(value, radix, culture)</h2>
<p>
Parses a string representing a floating point number in the given the radix (10 by default), taking into account any formatting rules followed by the given culture (or the current culture, if not specified).
</p>

<h2>jQuery.parseDate(value, formats, culture)</h2>
<p>
Parses a string representing a date into a JavaScript Date object, taking into account the given possible formats (or the given culture's set of default formats if not given). As before, the current culture is used if one is not specified.
<pre>
jQuery.culture = jQuery.culture.en;
jQuery.parseDate("1/2/2003"); // Thu Jan 02 2003
jQuery.culture = jQuery.culture.fr;
jQuery.parseDate("1/2/2003"); // Sat Feb 01 2003
</pre>
</p>

<h2 id="localize">jQuery.localize(key, culture, value)</h2>
<p>
Gets or sets a localized value. This function allows you to extend the information available to a particular culture, and to easily retrieve it without worrying about finding the most appropriate culture. For example, to define the word "translate" in French:
<pre>
jQuery.localize("translate", "fr", "traduire");
</pre>
The value may be any value you wish -- a string, number, object, etc. You can then define a grouping of localized values common to a feature, plugin, or application.
<pre>
jQuery.localize("myplugin", "fr", {
    foo: "foo",
    bar: "bar"
});

var obj = jQuery.localize("myplugin", "fr");
alert(obj.foo); // "foo"
</pre>
Note that localize() will find the closest match available per the same semantics as the jQuery.findClosestCulture function. If there is no match, the translation given is for the neutral culture, if any.
<pre>
jQuery.localize("myplugin", "", {
    foo: "foo (invariant)",
    bar: "bar (invariant)"
});
jQuery.localize("myplugin", "fr", {
    foo: "foo",
    bar: "bar"
});

jQuery.culture = jQuery.cultures["fr"];
alert(jQuery.localize("myplugin").foo); // foo

jQuery.culture = jQuery.cultures["fr-FR"];
alert(jQuery.localize("myplugin").foo); // foo

jQuery.culture = jQuery.cultures["es-MX"];
jQuery.culture = jQuery.cultures["fr-FR"];
alert(jQuery.localize("myplugin").foo); // foo (invariant)
</pre>
Also note that localize() does not require loading the culture information script. You may use localize() for localization purposes without utilizing the parsing and formatting functions which depend on the cultures. If you do use both, it does not matter what order you include them in, either may be first -- the jQuery.glob.&lt;code&gt;.js script, or your own script which uses localize().
</p>

<h2 id="extend">Utilizing and Extending Cultures</h2>
<p>
The culture information included with each culture is mostly necessary for the parsing and formatting methods, but not all of it. For example, the the Native and English names for each culture is given, as well as a boolean indicating whether the language is right-to-left. This may be useful information for your own purposes. You may also add to the culture information directly if so desired. It is important to do so in a way that handles the fact that the culture info may not be provided, may not be provided yet, or may already be provided. Using jQuery's extend() method, it is possible to define a culture in a way that both defines if it does not exist and adds to it if it does exist.
</p>
<p>
As an example, in the U.S., the word "billion" means the number 1,000,000,000 (9 zeros). But in other countries, that number is "1000 million" or a "milliard", and a billion is 1,000,000,000,000 (12 zeros). If you needed to provide functionality to your app or custom plugin that needed to know how many zeros are in a 'billion', you could extend the culture information as follows:
<pre>
// define culture information without overwriting any existing values 
jQuery.cultures.fr = jQuery.extend(true, {
    numberFormat: {
        billionZeros: 12
    }
}, jQuery.cultures.fr);
</pre>
Using this mechanism, the 'fr' culture will be created if it does not exist. And if it does, the given values will be added to it, taking care not to overwrite anything that is already defined (if you'd prefer to overwrite, you'd switch the last two arguments). When the jQuery.glob.fr.js script is eventually included, it too uses this technique, ensuring addition to the already-defined culture information.
</p>

<h2 id="defining">Defining Culture Information</h2>
<p>
Each culture is defined in its own script with the naming scheme jQuery.glob.&lt;code&gt;.js (along with its minified version, jQuery.glob.&lt;code&gt;.min.js). You may include any number of these scripts, making them available in the jQuery.cultures mapping. Including one of these scripts does NOT automatically make it the default culture selected with jQuery.culture.
</p>
<p>
The neutral culture that comes with jQuery.glob.js is heavily commented, describing the purpose of each of the fields defined by a culture. Note that every culture includes all of these fields, even if they are the same as the netural culture. However, the script uses jQuery's $.extend to copy from the neutral culture, so looking at the raw scripts will only show you what is different in that culture from the neutral culture. The neutral culture is listed here along with the comments:
<pre>
jQuery.cultures.invariant = {
    // A unique name for the culture in the form &lt;language code&gt;-&lt;country/region code&lt;
    name: "invariant",
    // the name of the culture in the english language
    englishName: "Invariant",
    // the name of the culture in its own language
    nativeName: "Invariant",
    // whether the culture uses right-to-left text
    isRTL: false,
    // 'language' is used for so-called "specific" cultures.
    // For example, the culture "es-CL" means "Spanish, in Chili".
    // It represents the Spanish-speaking culture as it is in Chili,
    // which might have different formatting rules or even translations
    // than Spanish in Spain. A "neutral" culture is one that is not
    // specific to a region. For example, the culture "es" is the generic
    // Spanish culture, which may be a more generalized version of the language
    // that may or may not be what a specific culture expects.
    // For a specific culture like "es-CL", the 'language' field refers to the
    // neutral, generic culture information for the language it is using.
    // This is not always a simple matter of the string before the dash.
    // For example, the "zh-Hans" culture is netural (Simplified Chinese).
    // And the 'zh-SG' culture is Simplified Chinese in Singapore, whose lanugage
    // field is "zh-CHS", not "zh".
    // This field should be used to navigate from a specific culture to it's
    // more general, neutral culture. If a culture is already as general as it 
    // can get, the language may refer to itself.
    language: "",
    // numberFormat defines general number formatting rules, like the digits in
    // each grouping, the group separator, and how negative numbers are displayed.
    numberFormat: {
        // [negativePattern]
        // Note, numberFormat.pattern has no 'positivePattern' unlike percent and currency,
        // but is still defined as an array for consistency with them.
        //  negativePattern: one of "(n)|-n|- n|n-|n -"
        pattern: ["-n"], 
        // number of decimal places normally shown
        decimals: 2,
        // string that separates number groups, as in 1,000,000
        ',': ",",
        // string that separates a number from the fractional portion, as in 1.99
        '.': ".",
        // array of numbers indicating the size of each number group.
        groupSizes: [3],
        // symbol used for positive numbers
        '+': "+",
        // symbol used for negative numbers
        '-': "-",
        percent: {
            // [negativePattern, positivePattern]
            //     negativePattern: one of "-n %|-n%|-%n|%-n|%n-|n-%|n%-|-% n|n %-|% n-|% -n|n- %"
            //     positivePattern: one of "n %|n%|%n|% n"
            pattern: ["-n %","n %"], 
            // number of decimal places normally shown
            decimals: 2,
            // array of numbers indicating the size of each number group.
            groupSizes: [3],
            // string that separates number groups, as in 1,000,000
            ',': ",",
            // string that separates a number from the fractional portion, as in 1.99
            '.': ".",
            // symbol used to represent a percentage
            symbol: "%"
        },
        currency: {
            // [negativePattern, positivePattern]
            //     negativePattern: one of "($n)|-$n|$-n|$n-|(n$)|-n$|n-$|n$-|-n $|-$ n|n $-|$ n-|$ -n|n- $|($ n)|(n $)"
            //     positivePattern: one of "$n|n$|$ n|n $"
            pattern: ["($n)","$n"],
            // number of decimal places normally shown
            decimals: 2,
            // array of numbers indicating the size of each number group.
            groupSizes: [3],
            // string that separates number groups, as in 1,000,000
            ',': ",",
            // string that separates a number from the fractional portion, as in 1.99
            '.': ".",
            // symbol used to represent currency
            symbol: "¤"
        }
    },
    // calendars defines all the possible calendars used by this culture.
    // There should be at least one defined with name 'standard', and is the default
    // calendar used by the culture.
    // A calendar contains information about how dates are formatted, information about
    // the calendar's eras, a standard set of the date formats,
    // translations for day and month names, and if the calendar is not based on the Gregorian
    // calendar, conversion functions to and from the Gregorian calendar.
    calendars: {
        standard: {
            // name that identifies the type of calendar this is
            name: "Gregorian_USEnglish",
            // separator of parts of a date (e.g. '/' in 11/05/1955)
            '/': "/",
            // separator of parts of a time (e.g. ':' in 05:44 PM)
            ':': ":",
            // the first day of the week (0 = Sunday, 1 = Monday, etc)
            firstDay: 0,
            days: [
                // full day names
                ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"],
                // abbreviated day names
                ["Sun","Mon","Tue","Wed","Thu","Fri","Sat"],
                // shortest day names
                ["Su","Mo","Tu","We","Th","Fr","Sa"]
            ],
            months: [
                // full month names (13 months for lunar calendars -- 13th month should be "" if not lunar)
                ["January","February","March","April","May","June","July","August","September","October","November","December",""],
                // abbreviated month names
                ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec",""]
            ],
            // AM and PM designators in one of these forms:
            // The usual view, and the upper and lower case versions
            //      [standard,lowercase,uppercase] 
            // The culture does not use AM or PM (likely all standard date formats use 24 hour time)
            //      null
            AM: ["AM", "am", "AM"],
            PM: ["PM", "pm", "PM"],
            eras: [
                // eras in reverse chronological order.
                // name: the name of the era in this culture (e.g. A.D., C.E.)
                // start: when the era starts in ticks, null if it is the earliest supported era.
                // offset: offset in years from gregorian calendar
                {"name":"A.D.","start":null,"offset":0}
            ],
            // when a two digit year is given, it will never be parsed as a four digit
            // year great than this year (in the appropriate era for the culture)
            twoDigitYearMax: 2029,
            // set of predefined date and time patterns used by the culture
            // these represent the format someone in this culture would expect
            // to see given the portions of the date that are shown.
            patterns: {
                // short date pattern
                d: "MM/dd/yyyy",
                // long date pattern
                D: "dddd, dd MMMM yyyy",
                // short time pattern
                t: "HH:mm",
                // long time pattern
                T: "HH:mm:ss",
                // long date, short time pattern
                f: "dddd, dd MMMM yyyy HH:mm",
                // long date, long time pattern
                F: "dddd, dd MMMM yyyy HH:mm:ss",
                // month/day pattern
                M: "MMMM dd",
                // month/year pattern
                Y: "yyyy MMMM",
                // S is a sortable format that does not vary by culture
                S: "yyyy\u0027-\u0027MM\u0027-\u0027dd\u0027T\u0027HH\u0027:\u0027mm\u0027:\u0027ss"
            }
            // optional fields for each calendar:
            /*
            monthsGenitive:
                Same as months but used when the day preceeds the month.
                Omit if the culture has no genitive distinction in month names.
                For an explaination of genitive months, see http://blogs.msdn.com/michkap/archive/2004/12/25/332259.aspx
            convert:
                Allows for the support of non-gregorian based calendars. This convert object is used to
                to convert a date to and from a gregorian calendar date to handle parsing and formatting.
                The two functions:
                    fromGregorian(date)
                        Given the date as a parameter, return an array with parts [year, month, day]
                        corresponding to the non-gregorian based year, month, and day for the calendar.
                    toGregorian(year, month, day)
                        Given the non-gregorian year, month, and day, return a new Date() object 
                        set to the corresponding date in the gregorian calendar.
            */
        }
    }
}
</pre>
</p>
<p>
Each culture can have several possible calendars. The calendar named "standard" is the default calendar used by that culture. You may change the calendar in use by setting the 'calendar' field. Take a look at the calendars defined by each culture by looking at the script or enumerating its calendars collection.
<pre>
// switch to a non-standard calendar
$.culture.calendar = $.culture.calendars.SomeOtherCalendar;
// back to the standard calendar
$.culture.calendar = $.culture.calendars.standard;
</pre>

</p>

<h2 id="numbers">Number Formatting</h2>
<p>
When formatting a number with format(), the main purpose is to convert the number into a human readable string using the culture's standard grouping and decimal rules. The rules between cultures can vary a lot. For example, in some cultures, the grouping of numbers is done unevenly. In the "te-IN" culture (Telugu in India), groups have 3 digits and then 2 digits. The number 1000000 (one million) is written as "10,00,000". Some cultures do not group numbers at all.
</p>
<p>
There are four main types of number formatting:
<ul>
<li><strong>n</strong> for number</li>
<li><strong>d</strong> for decimal digits</li>
<li><strong>p</strong> for percentage</li>
<li><strong>c</strong> for currency</li>
</ul>
Even within the same culture, the formatting rules can vary between these four types of numbers. For example, the expected number of decimal places may differ from the number format to the currency format. Each format token may also be followed by a number. The number determines how many decimals places to display for all the format types except decimal, for which it means the minimum number of digits to display, zero padding it if necessary. Also note that the way negative numbers are represented in each culture can vary, such as what the negative sign is, and whether the negative sign appears before or after the number. This is especially apparent with currency formatting, where many cultures use parentheses instead of a negative sign.
<pre>
// just for example -- will vary by culture
jQuery.format(123.45, "n"); // 123.45
jQuery.format(123.45, "n0"); // 123
jQuery.format(123.45, "n1"); // 123.5

jQuery.format(123.45, "d"); // 123
jQuery.format(12, "d3"); // 012

jQuery.format(123.45, "c"); // $123.45
jQuery.format(123.45, "c0"); // $123
jQuery.format(123.45, "c1"); // $123.5
jQuery.format(-123.45, "c"); // ($123.55)

jQuery.format(0.12345, "p"); // 12.35 %
jQuery.format(0.12345, "p0"); // 12 %
jQuery.format(0.12345, "p4"); // 12.3450 %
</pre>
Parsing with parseInt and parseFloat also accepts any of these formats.
</p>

<h2 id="dates">Date Formatting</h2>
<p>
Date formatting varies wildly by culture, not just in the spelling of month and day names, and the date separator, but by the expected order of the various date components, whether to use a 12 or 24 hour clock, and how months and days are abbreivated. Many cultures even include "genative" month names, which are different from the typical names and are used only in certain cases.
</p>
<p>
Also, each culture has a set of "standard" or "typical" formats. For example, in en-US, when displaying a date in its fullest form, it looks like "Saturday, November 05, 1955". Note the non-abbreivated day and month name, the zero padded date, and four digit year. So, jQuery.glob.js defines a certain set of "standard" formatting strings for dates that are aliases to the specific formats for the culture. See the 2nd table below for the meaning of each token in the en-US examples.
<table>
<tr>
  <th>Token</th>
  <th>Meaning</th>
  <th>en-US (example)</th>
</tr>
<tr>
   <td>f</td>
   <td>Long Date, Short Time</td>
   <td>dddd, MMMM dd, yyyy h:mm tt</td>
</tr>
<tr>
   <td>F</td>
   <td>Long Date, Long Time</td>
   <td>dddd, MMMM dd, yyyy h:mm:ss tt</td>
</tr>
<tr>
   <td>t</td>
   <td>Short Time</td>
   <td>h:mm tt</td>
</tr>
<tr>
   <td>T</td>
   <td>Long Time</td>
   <td>h:mm:ss tt</td>
</tr>
<tr>
   <td>d</td>
   <td>Short Date</td>
   <td>M/d/yyyy</td>
</tr>
<tr>
   <td>D</td>
   <td>Long Date</td>
   <td>dddd, MMMM dd, yyyy</td>
</tr>
<tr>
   <td>Y</td>
   <td>Month/Year</td>
   <td>MMMM, yyyy</td>
</tr>
<tr>
   <td>M</td>
   <td>Month/Day</td>
   <td>yyyy MMMM</td>
</tr>
</table>
</p>
<p>
In addition to these standard formats, there is the 'S' format. This is a sortable format that is always the same for every culture: <strong>yyyy'-'MM'-'dd'T'HH':'mm':'ss</strong>.
</p>
<p>
When more specific control is needed over the formatting, you may use any format you wish by specifing the the following custom tokens:
<table>
<tr>
   <th>Token</th>
   <th>Meaning</th>
   <th>Example</th>
</tr>
<tr>
   <td>d</td>
   <td>Day of month (no leading zero)</td>
   <td>5</td>
</tr>
<tr>
   <td>dd</td>
   <td>Day of month (leading zero)</td>
   <td>05</td>
</tr>
<tr>
   <td>ddd</td>
   <td>Day name (abbreviated)</td>
   <td>Sat</td>
</tr>
<tr>
   <td>dddd</td>
   <td>Day name (full)</td>
   <td>Saturday</td>
</tr>
<tr>
   <td>M</td>
   <td>Month of year (no leading zero)</td>
   <td>9</td>
</tr>
<tr>
   <td>MM</td>
   <td>Month of year (leading zero)</td>
   <td>09</td>
</tr>
<tr>
   <td>MMM</td>
   <td>Month name (abbreivated)</td>
   <td>Sept</td>
</tr>
<tr>
   <td>MMMM</td>
   <td>Month name (full)</td>
   <td>September</td>
</tr>
<tr>
   <td>yy</td>
   <td>Year (two digits)</td>
   <td>55</td>
</tr>
<tr>
   <td>yyyy</td>
   <td>Year (four digits)</td>
   <td>1955</td>
</tr>
<tr>
   <td>'literal'</td>
   <td>Literal Text</td>
   <td>'Hmmmm'...</td>
</tr>
<tr>
   <td>\'</td>
   <td>Single Quote</td>
   <td>&nbsp;</td>
</tr>
<tr>
   <td>m</td>
   <td>Minutes (no leading zero)</td>
   <td>9</td>
</tr>
<tr>
   <td>mm</td>
   <td>Minutes (leading zero)</td>
   <td>09</td>
</tr>
<tr>
   <td>h</td>
   <td>Hours (leading zero)</td>
   <td>6</td>
</tr>
<tr>
   <td>hh</td>
   <td>Hours (leading zero)</td>
   <td>06</td>
</tr>
<tr>
   <td>H</td>
   <td>Hours (24 hour time, no leading zero)</td>
   <td>5 (5am) 15 (3pm)</td>
</tr>
<tr>
   <td>HH</td>
   <td>Hours (24 hour time, leading zero)</td>
   <td>05 (5am) 15 (3pm)</td>
</tr>
<tr>
   <td>s</td>
   <td>Seconds (no leading zero)</td>
   <td>9</td>
</tr>
<tr>
   <td>ss</td>
   <td>Seconds (leading zero)</td>
   <td>09</td>
</tr>
<tr>
   <td>f</td>
   <td>Deciseconds</td>
   <td>1</td>
</tr>
<tr>
   <td>ff</td>
   <td>Centiseconds</td>
   <td>11</td>
</tr>
<tr>
   <td>fff</td>
   <td>Milliseconds</td>
   <td>111</td>
</tr>
<tr>
   <td>t</td>
   <td>AM/PM indicator (first letter)</td>
   <td>A or P</td>
</tr>
<tr>
   <td>tt</td>
   <td>AM/PM indicator (full)</td>
   <td>AM or PM</td>
</tr>
<tr>
   <td>z</td>
   <td>Timezone offset (hours only, no leading zero)</td>
   <td>-8</td>
</tr>
<tr>
   <td>zz</td>
   <td>Timezone offset (hours only, leading zero)</td>
   <td>-08</td>
</tr>
<tr>
   <td>zzz</td>
   <td>Timezone offset (full hours/minutes)</td>
   <td>-08:00</td>
</tr>
<tr>
   <td>g or gg</td>
   <td>Era name</td>
   <td>A.D.</td>
</tr>
</table>
</p>
