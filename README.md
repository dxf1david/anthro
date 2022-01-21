\name{anthro}
\alias{anthro}
\alias{age_in_months}
\alias{weight_kg}
\alias{height_cm}
\alias{ref_data}
\alias{nhanes}
\alias{data}
%- Also NEED an '\alias' for EACH other topic documented here.
\title{
GENERATE SEX- AND AGE-STANDARDIZED WEIGHT, HEIGHT, AND BMI METRICS FROM THE CDC GROWTH CHARTS
}
\description{
Generate z-scores, percentiles, and other metrics for weight, height, and BMI
based on the 2000 CDC growth charts.  Has a single function, 'anthro'.  Requires the package data.table to be installed
}
\usage{
anthro(data, age = age_in_months, wt = weight_kg, ht = height_cm, bmi = bmi)
# OR
# anthro(data, age_in_months, weight_kg, height_cm, bmi)
# Do NOT put arguments in quotation marks
}
%- maybe also 'usage' for other objects documented here.
\arguments{
  \item{age}{age in months specified as accuately as possible.}
  \item{wt}{weight (kg).}
  \item{ht}{height (cm).}
  \item{bmi}{BMI, kg/m^2.}
}
\details{lib
Expects sex to be coded as 1 (boys) or 2 (girls) in data.  Age in months should be given as accurately as possible because the function linearly interpolates between ages. If completed number of months is known (e.g., NHANES), add 0.5.

If age is in days, divide by 30.4375 so that a child who is 3672 days old would have an age in months of 120.641.
}
\value{
Returns a data.table containing the original data and various weight, height, and BMI metrics. Can convert this to a dataframe with 'setDF(output_data)'.


Variables in output:

waz, haz, bmiz: CDC --for-age z-scores for Weight, Height, and BMI

mod_waz, mod_haz, mod_bmiz: modified z-scores

ext_bmip and ext_bmiz: extended BMI percentile and z-score
}
\references{
Kuczmarski RJ, Ogden CL, Guo SS, Grummer-Strawn LM, Flegal KM, Mei Z, et al. 2000 CDC Growth Charts for the United States: methods and development. Vital and Health Statistics Series 11, Data from the National Health Survey 2002;11:1–190.

Wei R, Ogden CL, Parsons VL, Freedman DS, Hales CM. A method for calculating BMI z-scores and percentiles above the 95th percentile of the CDC growth charts. Annals of Human Biology 2020;47:514–21. https://doi.org/10.1080/03014460.2020.1808065.

Freedman DS, Woo JG, Ogden CL, Xu JH, Cole TJ. Distance and Percent Distance from Median BMI as Alternatives to BMI z-score. Br J Nutr 2019;124:1–8.
https://doi.org/10.1017/S0007114519002046.


}
\author{
David Freedman
}
\note{
Do NOT put arguments in quotation marks, such as anthro(data,'age','wt','ht','bmi').
Uses the the merged LMS data files at
https://www.cdc.gov/growthcharts/percentile_data_files.htm.
}

%% ~Make other sections like Warning with \section{Warning }{....} ~

\seealso{
https://www.cdc.gov/nccdphp/dnpao/growthcharts/resources/sas.htm
}
\examples{
data <- expand.grid(sex=1:2, agem=120.5, wtk=c(30,60), htc=c(135,144));
data$bmi = data$wt / (data$ht/100)^2;
data = anthro(data, age=agem, wt=wtk, ht=htc, bmi);
# OR data = anthro(data, agem, wtk, htc, bmi);
round(data,2)
# setDF(data) to convert to a dataframe

nhanes   # NHANES data (2015/16 and 2017/18)
nhanes$agemos = nhanes$agemos + 0.5   # because agemos is completed number of months
data = anthro(nhanes, age=agemos, wt, ht, bmi)
# OR data = anthro(nhanes, agemos, wt, ht, bmi)
round(data, 2)
}
% Add one or more standard keywords, see file 'KEYWORDS' in the
% R documentation directory (show via RShowDoc("KEYWORDS")):
% \keyword{ ~kwd1 }
% \keyword{ ~kwd2 }
% Use only one keyword per line.
% For non-standard keywords, use \concept instead of \keyword:
% \concept{ ~cpt1 }
% \concept{ ~cpt2 }
% Use only one concept per line.
