Why the State of Javascript/Typescript/React/Angular development is so complex.

How to calculate the 2 digit year efficiently.
| Method    | Mean      | Error    | StdDev   |
|---------- |----------:|---------:|---------:|
| UseMod    |  16.09 us | 0.319 us | 0.298 us |
| UseString | 461.91 us | 5.248 us | 4.909 us |

        [Benchmark]
        public void UseMod()
        {
            for (var value = rangeStart; value < rangeEnd; value++)
            {
                var byteYear = value % 100;
                var bytes = BitConverter.GetBytes(byteYear);
            }
        }

        [Benchmark]
        public void UseString()
        {
            for (var value = rangeStart; value < rangeEnd; value++)
            {
                var byteYear = Convert.ToInt32(value.ToString().PadLeft(4, '0').Substring(2, 2));
                var bytes = BitConverter.GetBytes(byteYear);
            }
        }
