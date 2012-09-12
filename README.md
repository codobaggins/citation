# Citation.js

A JavaScript library for detecting US Code citations, and other kinds of legal citations, in blocks of text.


### Example Usage

Calling:

	Citation.find(
		"(11) INTERNET- The term Internet has the meaning given " +
		"that term in section 5362(5) of title 31, United States Code. " +
		"All regulations in effect immediately before " +
		"the enactment of subsection (f) that were promulgated under " +
		"the authority of this section shall be repealed in accordance " +
		"with the Administrative Procedure Act (5 U.S.C. 552(a)(1)(E))"
	)

Returns:

	[{
		match: "5 U.S.C. 552(a)(1)(E)",
		type: "usc",
		usc: {
			title: "5",
			section: "552",
			subsections: ["a", "1", "E"]
			id: "5_usc_552_a_1_E",
			section_id: "5_usc_552",
			display: "5 USC 552(a)(1)(E)"
		}
	}, {
		match: "section 5362(5) of title 31",
		type: "usc",
		usc: {
			title: "31",
			section: "5362",
			subsections: ["5"],
			id: "31_usc_5362_5",
			section_id: "31_usc_5362",
			display: "31 USC 5362(5)"
		}
	}]


Note: Citations are not necessarily returned in the order they appear in the source text.


Pass an optional "context" value (a number) to get an excerpt with up to that number of characters on either side of each detected citation.

	Citation.find(
		"(11) INTERNET- The term Internet has the meaning given " +
		"that term in section 5362(5) of title 31, United States Code. " +
		"All regulations in effect immediately before " +
		"the enactment of subsection (f) that were promulgated under " +
		"the authority of this section shall be repealed in accordance " +
		"with of the Administrative Procedure Act (5 U.S.C. 552(a)(1)(E))",

		{context: 10}
	)

Returns:

	[{
		context: "dure Act (5 U.S.C. 552(a)(1)(E))",
		match: "5 U.S.C. 552(a)(1)(E)",
		type: "usc",
		usc: {
			title: "5",
			section: "552",
			subsections: ["a", "1", "E"]
			id: "5_usc_552_a_1_E",
			section_id: "5_usc_552",
			display: "5 USC 552(a)(1)(E)"
		}
	}, {
		context: "t term in section 5362(5) of title 31, United S",
		match: "section 5362(5) of title 31",
		type: "usc",
		usc: {
			title: "31",
			section: "5362",
			subsections: ["5"],
			id: "31_usc_5362_5",
			section_id: "31_usc_5362",
			display: "31 USC 5362(5)"
		}
	}]


### Current Status

Version 0.1.1, available in npm.

Under active development. Currently just uses simple pattern matching, for self-contained citations only.


### Real world examples

You can see Citation.js in action in the Sunlight Foundation's government search and alert service, [Scout](http://scout.sunlightfoundation.com).

For example, a search for ["5 usc 552"](https://scout.sunlightfoundation.com/search/federal_bills/5%20usc%20552) or ["section 601 of title 5"](https://scout.sunlightfoundation.com/search/federal_bills/section%20601%20of%20title%205) will return results matching multiple formats and subsections, with highlighted excerpts.

To accomplish this, bills and regulations are pre-processed in regular batches by a Ruby script that [submits their text](https://github.com/sunlightlabs/realtimecongress/blob/master/tasks/utils.rb#L17) to an instance of [citation-api](https://github.com/sunlightlabs/citation-api) and stores the extracted citations and excerpts, which are then exposed via API.


### HTTP API

A minimal Node.js wrapper over this library can be found at [citation-api](https://github.com/sunlightlabs/citation-api).


### Upcoming

* Many more US Code citation formats.
* More citation types: US bills, slip laws, Code of Federal Regulations.