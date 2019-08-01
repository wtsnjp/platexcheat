# Rakefile for wtsnjp/platexcheat
require 'rake/clean'
require 'pathname'
require 'optparse'

# basics
PKG_VERSION = "3.1"
PKG_NAME = "platexcheat-#{PKG_VERSION}"

# woking/temporaly dirs
PWD = Pathname.pwd
TMP_DIR = PWD + "tmp"

# cleaning
CLEAN.include(["*.aux", "*.dvi", "*.log", "*.synctex.gz", "tmp"])
CLOBBER.include(["*.pdf", "*.zip"])

# deault
task default: :doc

desc "Typeset all contents"
task :doc do
  sh "llmk > #{File::NULL} 2> #{File::NULL}"
end

desc "Create an archive for CTAN"
task :ctan => :doc do
  # initialize the target
  TARGET_DIR = TMP_DIR + PKG_NAME
  rm_rf TARGET_DIR
  mkdir_p TARGET_DIR

  # copy all required files
  pkg_files = ["LICENSE", "README.md", Dir.glob("*.tex"), Dir.glob("*.pdf")].flatten
  cp pkg_files, TARGET_DIR

  # add short English introduction to the README.md
  intro_en = <<~INTRO
    # pLaTeX2e cheat sheet

    This is a translation to Japanese of Winston Chang's LaTeX cheat sheet (a reference sheet for writing scientific papers). It has been adapted to Japanese standards using pLaTeX, and also attached additional information of "standard LaTeX" (especially about math-mode).
  INTRO

  File.open(TARGET_DIR + "README.md", "r+") do |f|
    content = f.read
    f.seek 0
    f.write intro_en + "\n"
    f.write content
  end

  # create zip archive
  cd TMP_DIR
  sh "zip -q -r #{PKG_NAME}.zip #{PKG_NAME}"
  mv "#{PKG_NAME}.zip", PWD
end
