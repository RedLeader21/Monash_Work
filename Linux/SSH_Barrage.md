#!/bin/bash
for i in {1..100}
do
   ssh test@10.0.0.5
   ssh test@10.0.0.6
   ssh test@10.0.0.9
done