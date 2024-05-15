Towels:

Commands:

Default Usage: tow $num_big $num_small
	Equivalent to 
		tow -i $LAST_USER_ID -b A -s B
	or
		tow -0 -b A -s B

Flags:
	tow -[i|e|n|p|s|x|h|l|q] OPTIONAL  {-b | -a}
	

	Search Flags:
	-i --id: Searches for users by user_id.
		Throws an error if no user is found
	-e, --email: Searches for users by user_email
		If no user is found, returns a fuzzy search with the input string.
		If more than one user is found, returns the list of users (see tow -l)
	-n, --name: Searches for users by user_name 
		If no user is found, returns a fuzzy search with the input string.
		If more than one user is found, returns the list of users (see tow -l)
	-p, --phone: Searches for users by user_phone
		If more than one user is found, returns the list of users (see tow -l)
	-s, --search: Fuzzily searches for users with the given string in all categories.
		Always returns the list case (see tow -l)
		Throws an error if no users are found.
	-x, --regex: Allows regular expressions when searching. 
		Can be used in conjunction with other search flags.
	-t	Returns a list of the last 10 valid users to check in, 
		with the most recent at 0.

	-l, --list: To be used immediately after a fuzzy search.
		To prevent accidental check-ins, the previous list results 
		are deleted after every tow
	-[0-9] 	Shorthand for tow -t | tow -l #, where # is number between 0 and  9. 
		Calls tow -i on the #th person to have checked in.
		tow (without flags) defaults to tow -0, which is 
		the most recent check in.
	-q, --status: Like -x, the -q flag can be used with other search flags.
		If no user is found, returns a fuzzy search with the input string.
		If more than one user is found, returns the list of users (see tow -l)
		When one user is found, returns some towel statistics, 
		including total towel debt and whether the user has checked out
		towels that day. 


	Misc Flags:
	-h, --help: Displays this message.
	

	Optional Flags:
	-b, --big [n]: Specifies the number of big towels checked out (or returned)
	-s, --small [n]: Specifies the number of small towels checked out (or returned)

Usage:

	For the rest of this document, we will assume that the 
	user is using tow -i. Using tow with any flag that retuns 
	a list and then choosing an element from that list is 
	equivalent to calling tow -i on that user_id.

	The average user should find tow -t and tow -l sufficient for check in purpose.
	For return purposes, tow -n or tow -s should be most effective.

	Some sample workflows are demonstrated here:
	Commands to the terminal are prefaced with a "$:"
	

	1: Standard Check-in + towels
		A member checks in and immediately asks for 2 large towels.
		$:tow 20
	tow without flags acts on the most recent check-in. 
	Since the shorthand usage is
	tow #B#S, tow 20 is 2 big towels and 0 small towels.
	The parsing means that calling tow 2 also checks out 
	two big towels, but we should avoid calling tow on 
	a single-digit number for clarity's sake.

	2: Standard Check-out
		A member drops off their towels and calls out their name
		$:tow -n "$member_name"
	For a user with checked in towels that day, tow -i user_id will
	execute the return sequence. Unless specified with addtional flags 
	(e.g., tow -i user_id 10 to show that the user did not return both
	big towels), tow -i user_id will return all towels checked out by the user.  
		 

Troubleshooting:
	
	These solutions were written before any code, 
	but they should generally suffice for users 
	who are new to the terminal and bash's peculiarities.

	tow -n returned a list but the person has a unique name
	-Due to the way bash processes commands, calling tow -n John Smith
	will call tow -n John
	To call tow -n John Smith, we need to enclose John Smith 
	in quotations as follows:
		tow -n "John Smith"
	
	tow -l checked in the wrong person
	calling tow -[0-9] -l will ignore the -l flag.
	Generally speaking, attempting to call tow with
	more than two flags will fail to have any effect
	other than calling tow with the first flag.


