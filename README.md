# bug-free-octo-waffle
class Integer:

    def __init__(self, bit_str=''):
        self.bit_list = []
        for i in range(len(bit_str)):
             integer = bit.Bit(int(bit_str[i]))
             self.bit_list.append(integer)
        if bit_str == '':
            self.bit_list.append(bit.Bit(0))
            
        
        
        
        
        ''' Create an integer from the bit_str.
        The bit_str is either empty or a string of "0"s and "1"s.
        The number of bits is determined by the length of the string. '''
        

    def __str__(self):
        string = ''
        for i in self.bit_list:
            string += str(i)
        return string
            
    
        ''' Return the string representation of this integer in binary. '''

    def __int__(self):
        result = 0
        x = 0
        for i in range(len(self.bit_list)-1,-1,-1):
            if str(self.bit_list[i]) == "1":
                result += 2**x
            x+=1
        return result
                
            
       # return int(self.str, 2)

    def __len__(self):
        return len(self.bit_list)
        ''' Return the number of bits in this Integer. '''

    def __invert__(self):
        inverse = Integer()
        inverse.bit_list = []
        for x in self.bit_list:
            inverse.bit_list.append(~x)
        return inverse
       
    
        ''' Return an Integer the inverse of this Integer (1's complement). '''

    def __ge__(self, other):
        return int(self) >= int(other)

    def __or__(self, other):
        number_str = ''
        self_str = str(self)
        other_str = str(other)
        if len(self_str) != len(other_str):
            raise ValueError("operands must be the same size")
        else:
            for i in range(len(self_str)):
                if self_str[i] == '0' and other_str[i] == '0':
                    number_str += '0'
                else:
                    number_str += '1'
            return number_str
        
        
        
            
        ''' Return an Integer with the bitwise OR of self and other.
        Both operands must be of the same length. '''

    def __and__(self, other):
        number_str = ''
        self_str = str(self)
        other_str = str(other)
        if len(self_str) != len(other_str):
            raise ValueError("operands must be the same size")
        else:
            for i in range(len(self_str)):
                if self_str[i] == '0' or other_str[i] == '0':
                    number_str += '0'
                else:
                    number_str += '1'
            return number_str
        ''' Return an Integer with the bitwise AND of self and other.
        Both operands must be of the same length. '''

    def __lshift__(self, i):
        self_str = str(self)
        new_str = ''
        new_str += self_str[i:]
        num_of_zero = len(self_str) - len(new_str)
        symbol = '0'
        return Integer(new_str + (symbol * num_of_zero))
            
            
        ''' Return the shift left of the Integer by i. 
        The answer has the same number of bits as the original. 
        The original Integer is unmodified. '''

    def __add__(self, other):
        bit.carry = bit.ZERO
        result = ''
        if len(str(self)) != len(str(other)) :
            raise ValueError("operands must be the same size")
        else:
            for i in range(len(self.bit_list)-1,-1,-1):
                one_bit = self.bit_list[i]
                other_bit = other.bit_list[i]
                add = one_bit.full_adder(other_bit)
                result += str(add)
            result = result[::-1]
            return Integer(result) 
                
        ''' Add self to other.
        Both operands must be of the same length.
        Return the result and set or clear the carry flag. '''

    def __mul__(self, other):
        full_length = len(self) + len(other)
        extra_self = Integer('0' * (full_length - len(self)) + str(self))
        extra_other = Integer('0' * (full_length - len(other)) + str(other))
        outcome = Integer('0' * full_length)
        shift_self = extra_self
        for i in range(len(extra_other)-1,-1,-1):
            if str(extra_other)[i] == '1':
                outcome = Integer(str(outcome + shift_self) )
            shift_self = shift_self << 1
        return outcome
                
    def __sub__(self, other):
        inverse_integer = ~other + Integer('0' * (len(other)-1) + '1')
        return self + inverse_integer
                                           
                
        ''' Subtract the bits from other from self.
        Both operands must be of the same length. '''

    def __divmod__(self, other):
        denominator = other
        quotient = Integer('0' * len(self))
        remnant = Integer('0' * len(other))
        for i in range(0,len(self.bit_list)):
            remnant = remnant << 1
            remnant.bit_list[-1] = self.bit_list[i]
            if remnant >= denominator:
                remnant = remnant - denominator
                quotient.bit_list[i] = bit.Bit(1)
        return(quotient, remnant)

        ''' Return the result of unsigned dividing self by other. 
        Both the quotient and the remainder.
        Operands may be of different lengths.'''

import bit
a = Integer('101')
b = Integer('101')

