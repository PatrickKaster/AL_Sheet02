#!/usr/bin/env python
import string
import random
import sys
from optparse import OptionParser

""" author: Patrick Kaster (kaster@cs.uni-bonn.de) """

class CellularAutomaton:
	"""
	        defines a cellular automaton with 84 cells, 2 states
		and an r neighbourhood, a rule in Wolfram notation and
		an inititialstate (S for seed, R for random)
	"""
	def __init__(self, r, rule, initialState, cellID=42):
		self.r = r

	  	# initialize 84 cells	
		self.cells = [0]*84
		self.initState(initialState, cellID)
		
		# initialize rule set	
		self.rule = self.initRule(rule)

	def run(self, maxIterations=0, symbolic=True):
		""" set maxIterations > 0 to stop after a number of iterations """
		
		self.printState(self.cells, symbolic) # outputs result of initialization		
		j = 0		
		while(True):
			if ( (maxIterations > 0) & (j > maxIterations) ):
				sys.exit(0)			
			for i in range(2, len(self.cells)-2):
				self.cells[i] = self.determineState(self.cells[i-self.r:i+self.r+1])
			
			self.printState(self.cells, symbolic)
			j+=1

	def determineState(self, neighbourhood):
		""" determines which state to apply to cell i according to neighbourhood """
		if neighbourhood in self.rule[0]:
			return 0
		elif neighbourhood in self.rule[1]:
			return 1
		
		print "something went wrong, neighbourhood not defined:", neighbourhood
		sys.exit(1)

	def initState(self, initialState, cellID):
		
		if initialState == 'S':
			self.cells[cellID] = 1		
			return
		
		if initialState == 'R':
					
			for i in range(2, len(self.cells)-2):
				self.cells[i] = random.randint(0, 1)
			return

                
	def initRule(self, wolframNumber):
		""" inits the rule as two lists of configurations for the two states """
		rule = []		
		zeroSet = []
		oneSet = []

		bitlength = 2 ** (2*self.r+1)-1		
		maxNumber = 2 ** (bitlength)-1

		if ( wolframNumber > maxNumber ):
			print "rule can't be realized with this r, try bigger r!"
			sys.exit(1)

		wolframNumberString = str(bin(wolframNumber)).replace('0b', '') # strip binary indicator
		wolframNumberString = wolframNumberString.zfill(bitlength+1)
		
		# change endianess for simpler parsing		
		wolframNumberL = list(wolframNumberString)	
		wolframNumberL.reverse()

		# push neighbourhood patterns to 0 or 1 set, according to bit in binary wolfram number		
		for i in range (0, len(wolframNumberL)):
			binaryPattern = str(bin(i)).replace('0b', '') # strip binary indicator		
			binaryPattern = binaryPattern.zfill(2*self.r+1)
			binaryPattern = list(binaryPattern)
			binaryPattern = [int(x) for x in binaryPattern]
			if wolframNumberL[i] == '0':
				zeroSet.append(binaryPattern)
			elif wolframNumberL[i] == '1':
				oneSet.append(binaryPattern)
			
		# form rule from both configuration sets		
		rule.append(zeroSet)
		rule.append(oneSet)

		return rule

	
	def printState(self, cells, symbolic=True):
		""" 
			pretty prints the current state of the automaton
		    	if symbolic is set to False states will be output as 0,1		
		"""
		cellString = str(cells).strip('[]')
		cellString = cellString.replace(', ', '')

		if symbolic:		
			cellString = cellString.replace('0', '_')
			cellString = cellString.replace('1', '#')
		
		print cellString


	

if __name__ == "__main__":

    	desc="""
		%prog [options]\n - %prog: a 1-D, two state cellular automaton able to read rules in Wolfram notation
	     """


	parser = OptionParser(desc)
    	parser.add_option("-r", dest="r", help="r (for neighbourhood radius), either 1 or 2", metavar="NUMBER", type="int", default=1)
    	parser.add_option("-n", "--rule", dest="rule", help="(decimal) rule number in Wolfram notation", metavar="NUMBER", type="int", default=90)
	parser.add_option("-S", "--startingCondition", dest="initialState", help="\'S\' for seeding cellID to \'1\'/\'set\' or \'R\' for random", metavar="CHAR", type="string", default='S')
	parser.add_option("-i", "--cellID", dest="cellID", help="cellID from 2 to 81 to use as seed, if -S is set to \'S\', defaults to 42", metavar="NUMBER", type="int")
	parser.add_option("-I", "--Iterations", dest="maxIterations", help="stop after number of iterations, if not set will run in an endless loop", metavar="NUMBER", type="int", default=0)
	parser.add_option("-p", "--non-symbolic", dest="nonSymbolic", help="non-symbolic: print state of cells as \'0\'/\'1\' instead of using symbols", metavar="Y/N", choices=['Y', 'N'], type="choice", default='N')
   	(options, args) = parser.parse_args()
		
	
	if (options.r > 2) | (options.r < 1):
		parser.error("neighbourhood radius is either 1 or 2")
	
	if ( (options.cellID > 1) & (options.cellID < 82) ):
		automaton = CellularAutomaton(options.r, options.rule, options.initialState, options.cellID)
	else:
		automaton = CellularAutomaton(options.r, options.rule, options.initialState)
	
	if (options.nonSymbolic == 'Y'):
		automaton.run(options.maxIterations, False)
	else:
		automaton.run(options.maxIterations)
