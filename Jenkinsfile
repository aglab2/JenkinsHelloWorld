#!/usr/bin/env groovy

timestamps {
	stage('build') {
		parallel(
			"build_win": {
				node("windows") {
					checkout scm
					String vsvars_bat = 'C:\\Program Files (x86)\\Microsoft Visual Studio 12.0\\VC\\vcvarsall.bat'
					bat """
						call "%ProgramFiles(X86)%\\%{vsvars_bat}" x86
						cl.exe hello.c
					"""
					stash name: "build_win", includes: "*.exe"
				}
			}
		)
	}
	
	stage('store') {
	}
}