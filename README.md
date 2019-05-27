# InvestecTestApp
Hi I am Nokuthula Ndlovu
package HighestCommonFactor;

import java.util.Arrays;

    public class CommonFactor {
        public static void main(String[] args) {

            CommonFactor commonFactor = new CommonFactor();

            // Given array from the link
            int [] numArray = new int[]{54, 24};
            commonFactor.highestCommonFactor(numArray);
        }

        public int highestCommonFactor(int[] numbers) {

            //1.  Find the highest common factor
            // (https://en.wikipedia.org/wiki/Greatest_common_divisor)
            // for a given array of integers



            Arrays.sort(numbers);
            int answer = numbers[0];
            int count = 1;
            while (count < numbers.length){
                if (numbers[count] % answer == 0) {
                    count++;
                } else {
                    answer = (numbers[count]%answer) ;
                    count++;
                }
            }


            System.out.println("The Highest common factor is = " + answer);
            return answer;
        }

}
[TestApp.zip](https://github.com/Sweeteeloh/InvestecTestApp/files/3225020/TestApp.zip)
