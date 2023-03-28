```r
write.csv(Mfitallo$aov.table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/MANOVA_mean_all.csv", row.names=TRUE)
write.csv(Mfitallo.ac$aov.table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/MANOVA_mean_actual.csv", row.names=TRUE)
write.csv(Mfitallo.extantonly$aov.table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/MANOVA_mean_extantonly.csv", row.names=TRUE)
write.csv(Mfitallo.pgls$aov.table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/MANOVA_mean_pgls.csv", row.names=TRUE)

write.csv(HOS_test$table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/HOStest_mean_extant.csv", row.names=TRUE)

write.csv(slope_angle$summary.table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/PWtest-angles_mean_extant.csv", row.names=TRUE)
write.csv(slopea_corB,"/home/lea.dacunha/Work/M2/R/tables/final_tables/angle_bonferroni_correction.csv", row.names=TRUE)
write.csv(slopea_corHol,"/home/lea.dacunha/Work/M2/R/tables/final_tables/angle_holm_correction.csv", row.names=TRUE)

write.csv(slope_length$summary.table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/PWtest-distance_mean_extant.csv", row.names=TRUE)
write.csv(sloped_corB,"/home/lea.dacunha/Work/M2/R/tables/final_tables/distance_bonferroni_correction.csv", row.names=TRUE)
write.csv(sloped_corHol,"/home/lea.dacunha/Work/M2/R/tables/final_tables/distance_holm_correction.csv", row.names=TRUE)

write.csv(coord.mod$table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allo_confusion_matrix.csv", row.names=TRUE)
write.csv(as.data.frame(coord.mod$overall),"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allo_stats.csv", row.names=TRUE)
write.csv(as.data.frame(coord.mod$byClass),"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allo_class_stats.csv", row.names=TRUE)
write.csv(pfoss$posterior,"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allofoss_pred.csv", row.names=TRUE)

write.csv(coordaf.mod$table,"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allofree_confusion_matrix.csv", row.names=TRUE)
write.csv(as.data.frame(coordaf.mod$overall),"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allofree_stats.csv", row.names=TRUE)
write.csv(pfossaf$posterior,"/home/lea.dacunha/Work/M2/R/tables/final_tables/lda_allofreefoss_pred.csv", row.names=TRUE)
```