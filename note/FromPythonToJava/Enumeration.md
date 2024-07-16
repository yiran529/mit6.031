Sometimes a type has a small, finite set of immutable values, such as:

months of the year: January, February, …, November, December
days of the week: Monday, Tuesday, …, Saturday, Sunday
compass points: north, south, east, west
available colors: black, gray, red, …
When the set of values is small and finite, it makes sense to define all the values as named constants, which Java calls an enumeration and expresses with the enum construct.

public enum Month { 
    JANUARY, FEBRUARY, MARCH, APRIL, 
    MAY, JUNE, JULY, AUGUST, 
    SEPTEMBER, OCTOBER, NOVEMBER, DECEMBER;
}

public enum PenColor { 
    BLACK, GRAY, RED, PINK, ORANGE, 
    YELLOW, GREEN, CYAN, BLUE, MAGENTA;
}
You can use an enumeration type name like PenColor in a variable or method declaration:

PenColor drawingColor;
Refer to the values of the enumeration as if they were named static constants:

drawingColor = PenColor.RED;
Note that an enumeration is a distinct new type. Older languages, like Python 2 and early versions of Java, tend to use numeric constants or string literals to represent a finite set of values like this. But an enumeration is more typesafe, because it can catch mistakes like type mismatches:

int month = TUESDAY;  //  no error if integers are used
Month month = DayOfWeek.TUESDAY; // static error if enumerations are used 
or misspellings:

String color = "REd";        // no error, misspelling isn't caught
PenColor drawingColor = PenColor.REd; // static error when enumeration value is misspelled