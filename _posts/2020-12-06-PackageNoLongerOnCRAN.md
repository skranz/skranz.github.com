---
layout: post
title: "What to do if a package you need is no longer on CRAN?"
cover: 
date: 2020-12-06 12:00:00
categories: r
tags: [R]
---

Currently, I am teaching a new course *Empirical Economics with R*. The [RTutor](https://github.com/skranz/RTutor) problem set for the chapter on Difference-in-Difference estimation uses the great package [lfe](https://cran.r-project.org/web/packages/lfe/index.html). I consider it an essential package to convince Stata users (still most empirical economists) to move to R, because it allows to combine with a single function call fast fixed effects estimation, use of instrumental variables and cluster-robust standard errors. 

However, yesterday a student send me an email that he could not install the `lfe` package because it was no longer on CRAN. The CRAN page looked as follows:

<hr>
<center>
<img src="http://skranz.github.io/images/cran/lfe_missing.PNG" style="max-width: 100%; margin-bottom: 0.5em;">
</center>
<hr>

While there is a link to an [archive](https://cran.r-project.org/src/contrib/Archive/lfe/), it only contains source versions of the package. The `lfe` package has C code, i.e. installing from source would e.g under Windows require to install RTools. However, I really would prefer not having to ensure that all my (non-computer science) students with diverse OS have the proper toolchain to compile such packages from source.

### The short-term rescue: Microsoft's CRAN snapshots

First, I came up with the more complicated solution to create binary versions for different OS using [r-hub](https://builder.r-hub.io/) and then host those binary `lfe` packages on my own [drat-powered](https://cran.r-project.org/web/packages/drat/index.html) and R repository on Github.

Yet, then I found a much simpler solution: the daily [Microsoft's CRAN snapshots](https://blog.revolutionanalytics.com/2019/05/cran-snapshots-and-you.html). [Here](https://mran.microsoft.com/snapshot/2020-12-04/web/packages/lfe/index.html) is the archived CRAN page from 2020-12-04 when `lfe` was still on CRAN. We can install that version by simply running:


```r
install.packages("lfe",repos=unique(c(
    getOption("repos"),
    repos="https://cran.microsoft.com/snapshot/2020-12-04/"
)))
```

This code first checks the default CRAN repository (should `lfe` reappear) and otherwise uses Microsoft's snapshot.

### What if there is a new major R version?

It might be that Microsoft's CRAN snapshots work nicely only until the day there is a new major R version, because new major R versions tend to require newly compiled binaries. If one wants to use the package for a course afterwards, one may have to restort to host an own [drat-powered](https://cran.r-project.org/web/packages/drat/index.html) repository using [r-hub](https://builder.r-hub.io/) to create the appropriate binaries for different OS. (Hint: If you use the r-hub web interface, you have to click on the *artifacts* link in the email from r-hub after a successful build to access the created binaries.)

### A hope for the future: A less stringent CRAN alternative

If one looks at the [CRAN check results](https://cran-archive.r-project.org/web/checks/2020/2020-12-04_check_results_lfe.html) for the lfe package from 2020-12-04, my interpretation (possibly wrong) is that the only problem seem to have been a *note* (not a warning or error) only for the compilation on *Solaris*:

<div style="margin-left: 5em">
<h4>CRAN Package Check Results for Package <a href="../packages/lfe/index.html"> lfe </a> </h4>
<p>
Last updated on 2020-12-04 09:50:22 CET.
</p>
<table border="1" summary="CRAN check results for package lfe">
<tr> <th> Flavor </th> <th> Version </th> <th> T<sub>install</sub> </th> <th> T<sub>check</sub> </th> <th> T<sub>total</sub> </th> <th> Status </th> <th> Flags </th> </tr>
<tr> <td>  <a href="check_flavors.html#r-devel-linux-x86_64-debian-clang"> r-devel-linux-x86_64-debian-clang </a> </td> <td> 2.8-5.1 </td> <td class="r"> 21.10 </td> <td class="r"> 227.73 </td> <td class="r"> 248.83 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-devel-linux-x86_64-debian-clang/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-devel-linux-x86_64-debian-gcc"> r-devel-linux-x86_64-debian-gcc </a> </td> <td> 2.8-5.1 </td> <td class="r"> 14.36 </td> <td class="r"> 181.05 </td> <td class="r"> 195.41 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-devel-linux-x86_64-debian-gcc/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-devel-linux-x86_64-fedora-clang"> r-devel-linux-x86_64-fedora-clang </a> </td> <td> 2.8-5.1 </td> <td class="r">  </td> <td class="r">  </td> <td class="r"> 308.05 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-devel-linux-x86_64-fedora-clang/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-devel-linux-x86_64-fedora-gcc"> r-devel-linux-x86_64-fedora-gcc </a> </td> <td> 2.8-5.1 </td> <td class="r">  </td> <td class="r">  </td> <td class="r"> 302.43 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-devel-linux-x86_64-fedora-gcc/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-devel-windows-ix86_x86_64"> r-devel-windows-ix86+x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r"> 50.00 </td> <td class="r"> 389.00 </td> <td class="r"> 439.00 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-devel-windows-ix86+x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-patched-linux-x86_64"> r-patched-linux-x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r"> 18.05 </td> <td class="r"> 223.84 </td> <td class="r"> 241.89 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-patched-linux-x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-patched-solaris-x86"> r-patched-solaris-x86 </a> </td> <td> 2.8-5.1 </td> <td class="r">  </td> <td class="r">  </td> <td class="r"> 321.80 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-patched-solaris-x86/lfe-00check.html">NOTE</a> </td> <td> --no-vignettes </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-release-linux-x86_64"> r-release-linux-x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r"> 17.55 </td> <td class="r"> 221.40 </td> <td class="r"> 238.95 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-release-linux-x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-release-macos-x86_64"> r-release-macos-x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r">  </td> <td class="r">  </td> <td class="r">  </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-release-macos-x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-release-windows-ix86_x86_64"> r-release-windows-ix86+x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r"> 37.00 </td> <td class="r"> 305.00 </td> <td class="r"> 342.00 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-release-windows-ix86+x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-oldrel-macos-x86_64"> r-oldrel-macos-x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r">  </td> <td class="r">  </td> <td class="r">  </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-oldrel-macos-x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
<tr> <td>  <a href="check_flavors.html#r-oldrel-windows-ix86_x86_64"> r-oldrel-windows-ix86+x86_64 </a> </td> <td> 2.8-5.1 </td> <td class="r"> 33.00 </td> <td class="r"> 369.00 </td> <td class="r"> 402.00 </td> <td> <a href="https://www.R-project.org/nosvn/R.check/r-oldrel-windows-ix86+x86_64/lfe-00check.html"><span class="check_ok">OK</span></a> </td> <td>  </td> </tr>
</table>
<h3>Check Details</h3>

<p>

Version: 2.8-5.1


<br/>

Flags: --no-vignettes


<br/>

Check: compilation flags used


<br/>

Result: NOTE


<br/>

&nbsp;&nbsp;&nbsp;&nbsp;Compilation used the following non-portable flag(s):<br/>
&nbsp;&nbsp;&nbsp;&nbsp;  ‘-mt’

<br/>

Flavor: <a href="https://www.r-project.org/nosvn/R.check/r-patched-solaris-x86/lfe-00check.html">r-patched-solaris-x86</a>

</p>
</div>

There are probably different opinions, but it seems to me a very tough decision to remove an extremely useful package from CRAN just because there was such a note for Solaris.

Do we really have to force statisticians who write very useful packages to dig deeply into compiler intricacies for a operating system probably most users never have heard about, just to remove a note?

And must all package users suffer if such a note cannot be removed (for whatever reason, e.g. time constraints of the package maintainer)?

Of course, the CRAN maintainers really do a lot of great work for the community and spend immense time ressources. So, of course, it is their right to specify the CRAN guidelines as they want and in a way that makes CRAN still manageable.

Still, personally, I would love if either CRAN would become less stringent or some organisation (RStudio perhaps?) would some day support an alternative repository with less stringent standards and more concerns about the trouble generated for users when removing a package just because of a compiler note for Solaris.

Until then let's hope such issues get quickly resolved so that such important packages find their way back to CRAN. 
