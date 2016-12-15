---
layout: post
title:  "Java Generic Best Practise"
date: 2015-12-11 23:00:00
categories: Java Best Practise
---

## Creating and Using Generic

	___Type Parameter___

	
	___Parameterized Type___


	___WildCard ?___


	___Bounded WildCard___


	___Raw type___
	In proper generic code, Collection would always be accompanied by a type parameter. When a generic type like Collection is used without a type parameter, it's called a raw type.
	So raw types are very much like wildcard types, but they are not typechecked as stringently. This is a deliberate design decision, to allow generics to interoperate with pre-existing legacy code.

	

## References
1. https://docs.oracle.com/javase/tutorial/java/generics/methods.html
2. http://www.angelikalanger.com/GenericsFAQ/FAQSections/ParameterizedTypes.html
3. (Taming Wildcards in Java Type system)[http://www.cs.cornell.edu/~ross/publications/tamewild/]


